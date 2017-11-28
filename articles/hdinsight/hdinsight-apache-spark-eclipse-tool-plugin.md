---
title: "aaaAzure Toolkit för Eclipse - skapa Scala program för HDInsight Spark | Microsoft Docs"
description: "Använda HDInsight Tools i Azure Toolkit för Eclipse toodevelop Spark-program som skrivits i Scala och skicka dem tooan HDInsight Spark-kluster, direkt från hello Eclipse IDE."
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
ms.openlocfilehash: 3ab70857c1e81f591a1c7e29bc1706ec4899ff58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-eclipse-toocreate-spark-applications-for-an-hdinsight-cluster"></a><span data-ttu-id="ba70d-103">Använd Azure Toolkit för Eclipse toocreate Spark-program för ett HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="ba70d-103">Use Azure Toolkit for Eclipse toocreate Spark applications for an HDInsight cluster</span></span>

<span data-ttu-id="ba70d-104">Använda HDInsight Tools i Azure Toolkit för Eclipse toodevelop Spark-program som skrivits i Scala och skicka dem tooan Azure HDInsight Spark-kluster, direkt från hello Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="ba70d-104">Use HDInsight Tools in Azure Toolkit for Eclipse toodevelop Spark applications written in Scala and submit them tooan Azure HDInsight Spark cluster, directly from hello Eclipse IDE.</span></span> <span data-ttu-id="ba70d-105">Du kan använda hello HDInsight Tools-plugin-programmet på några olika sätt:</span><span class="sxs-lookup"><span data-stu-id="ba70d-105">You can use hello HDInsight Tools plug-in in a few different ways:</span></span>

* <span data-ttu-id="ba70d-106">toodevelop och skicka Scala Spark-program på ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="ba70d-106">toodevelop and submit a Scala Spark application on an HDInsight Spark cluster</span></span>
* <span data-ttu-id="ba70d-107">tooaccess resurserna i Azure HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="ba70d-107">tooaccess your Azure HDInsight Spark cluster resources</span></span>
* <span data-ttu-id="ba70d-108">toodevelop och köra Scala Spark programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="ba70d-108">toodevelop and run a Scala Spark application locally</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ba70d-109">Det här verktyget kan använda toocreate och skicka program bara för ett HDInsight Spark-kluster på Linux.</span><span class="sxs-lookup"><span data-stu-id="ba70d-109">This tool can be used toocreate and submit applications only for an HDInsight Spark cluster on Linux.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="ba70d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ba70d-110">Prerequisites</span></span>

* <span data-ttu-id="ba70d-111">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ba70d-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="ba70d-112">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="ba70d-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="ba70d-113">Oracle Java Development Kit version 8, som används för hello Eclipse IDE runtime.</span><span class="sxs-lookup"><span data-stu-id="ba70d-113">Oracle Java Development Kit version 8, which is used for hello Eclipse IDE runtime.</span></span> <span data-ttu-id="ba70d-114">Du kan ladda ned det från hello [Oracle webbplats](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="ba70d-114">You can download it from hello [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="ba70d-115">Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="ba70d-115">Eclipse IDE.</span></span> <span data-ttu-id="ba70d-116">Den här artikeln använder Eclipse Neon.</span><span class="sxs-lookup"><span data-stu-id="ba70d-116">This article uses Eclipse Neon.</span></span> <span data-ttu-id="ba70d-117">Du kan installera den från hello [Eclipse webbplats](https://www.eclipse.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="ba70d-117">You can install it from hello [Eclipse website](https://www.eclipse.org/downloads/).</span></span>   
* <span data-ttu-id="ba70d-118">Spark SDK.</span><span class="sxs-lookup"><span data-stu-id="ba70d-118">Spark SDK.</span></span> <span data-ttu-id="ba70d-119">Du kan ladda ned det från [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="ba70d-119">You can download it from [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).</span></span>


## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-scala-plugin"></a><span data-ttu-id="ba70d-120">Installera HDInsight-verktyg i Azure Toolkit för Eclipse och Scala-plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="ba70d-120">Install HDInsight Tools in Azure Toolkit for Eclipse and Scala Plugin</span></span>
### <a name="install-hdinsight-tools"></a><span data-ttu-id="ba70d-121">Installera HDInsight-verktyg</span><span class="sxs-lookup"><span data-stu-id="ba70d-121">Install HDInsight Tools</span></span>
<span data-ttu-id="ba70d-122">HDInsight-verktyg för Eclipse är tillgänglig som en del av Azure Toolkit för Eclipse.</span><span class="sxs-lookup"><span data-stu-id="ba70d-122">HDInsight Tools for Eclipse is available as part of Azure Toolkit for Eclipse.</span></span> <span data-ttu-id="ba70d-123">Installationsanvisningar finns i [installerar Azure Toolkit för Eclipse](../azure-toolkit-for-eclipse-installation.md).</span><span class="sxs-lookup"><span data-stu-id="ba70d-123">For installation instructions, see [Installing Azure Toolkit for Eclipse](../azure-toolkit-for-eclipse-installation.md).</span></span>
### <a name="install-scala-plugin"></a><span data-ttu-id="ba70d-124">Installera Scala plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="ba70d-124">Install Scala Plugin</span></span>
<span data-ttu-id="ba70d-125">När du öppnar hello Intellij identifierar hello HDInsight Tools automatiskt om du har installerat Scala plugin-programmet eller inte.</span><span class="sxs-lookup"><span data-stu-id="ba70d-125">When you open hello Intellij, hello HDInsight Tools auto detects whether you installed Scala plugin or not.</span></span> <span data-ttu-id="ba70d-126">Klicka på **OK** toocontinue och följ hello instruktioner tooinstall av hello Eclipse Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ba70d-126">Click **OK** toocontinue and follow hello instructions tooinstall by hello Eclipse Marketplace.</span></span>

 ![Automatisk installation Scala plugin-program](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala.png)

## <a name="sign-in-tooyour-azure-subscription"></a><span data-ttu-id="ba70d-128">Logga in tooyour Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ba70d-128">Sign in tooyour Azure subscription</span></span>
1. <span data-ttu-id="ba70d-129">Starta hello Eclipse IDE och öppna Utforskaren i Azure.</span><span class="sxs-lookup"><span data-stu-id="ba70d-129">Start hello Eclipse IDE and open Azure Explorer.</span></span> <span data-ttu-id="ba70d-130">På hello **fönstret** -menyn klickar du på **visa**, och klicka sedan på **andra**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-130">On hello **Window** menu, click **Show View**, and then click **Other**.</span></span> <span data-ttu-id="ba70d-131">Expandera i hello dialogrutan som öppnas, **Azure**, klickar du på **Azure Explorer**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-131">In hello dialog box that opens, expand **Azure**, click **Azure Explorer**, and then click **OK**.</span></span>

    ![Visa dialogrutan vy](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. <span data-ttu-id="ba70d-133">Högerklicka på hello **Azure** noden och klicka sedan på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-133">Right-click hello **Azure** node, and then click **Sign in**.</span></span>
3. <span data-ttu-id="ba70d-134">I hello **Azure logga In** dialogrutan Välj autentiseringsmetod för hello, klickar du på **logga in**, och ange dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="ba70d-134">In hello **Azure Sign In** dialog box, choose hello authentication method, click **Sign in**, and enter your Azure credentials.</span></span>
   
    ![Azure logga In dialogrutan](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. <span data-ttu-id="ba70d-136">När du har loggat in hello **Välj prenumerationer** listar dialogrutan alla hello Azure-prenumerationer som är kopplade till hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ba70d-136">After you're signed in, hello **Select Subscriptions** dialog box lists all hello Azure subscriptions associated with hello credentials.</span></span> <span data-ttu-id="ba70d-137">Klicka på **Välj** tooclose hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ba70d-137">Click **Select** tooclose hello dialog box.</span></span>

    ![Dialogrutan för prenumerationer](./media/hdinsight-apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. <span data-ttu-id="ba70d-139">På hello **Azure Explorer** fliken, expandera **HDInsight** toosee hello HDInsight Spark-kluster i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ba70d-139">On hello **Azure Explorer** tab, expand **HDInsight** toosee hello HDInsight Spark clusters under your subscription.</span></span>
   
    ![HDInsight Spark-kluster i Azure Explorer](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. <span data-ttu-id="ba70d-141">Ytterligare kan du expandera en namnet nod toosee hello klusterresurser (till exempel storage-konton) som är associerade med hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="ba70d-141">You can further expand a cluster name node toosee hello resources (for example, storage accounts) associated with hello cluster.</span></span>
   
    ![Expandera en klusterresurser namn toosee](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)



## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a><span data-ttu-id="ba70d-143">Ställ in ett Spark Scala-projekt för ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="ba70d-143">Set up a Spark Scala project for an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="ba70d-144">Hello Eclipse IDE arbetsytan klickar **filen**, klickar du på **ny**, och klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-144">In hello Eclipse IDE workspace, click **File**, click **New**, and then click **Project**.</span></span> 
2. <span data-ttu-id="ba70d-145">Hello nytt projekt i guiden expanderar **HDInsight**väljer **Spark i HDInsight (Scala)**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-145">In hello New Project wizard, expand **HDInsight**, select **Spark on HDInsight (Scala)**, and then click **Next**.</span></span>

    ![Att välja hello Spark på HDInsight (Scala)-projekt](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. <span data-ttu-id="ba70d-147">Hej Scala projekt skapa guiden automatiskt identifierar om du har installerat Scala plugin-programmet eller inte.</span><span class="sxs-lookup"><span data-stu-id="ba70d-147">hello Scala project creation wizard auto detects whether you installed Scala plugin or not.</span></span> <span data-ttu-id="ba70d-148">Klicka på **OK** toocontinue hämtar hello Scala plugin-program och sedan följa hello instruktioner toorestart Eclipse.</span><span class="sxs-lookup"><span data-stu-id="ba70d-148">Click **OK** toocontinue downloading hello Scala plugin, then follow hello instructions toorestart Eclipse.</span></span>

    ![Kontrollera scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
4. <span data-ttu-id="ba70d-150">I hello **nytt projekt för HDInsight Scala** dialogrutan hello följande värden, och klickar sedan på **nästa**:</span><span class="sxs-lookup"><span data-stu-id="ba70d-150">In hello **New HDInsight Scala Project** dialog box, provide hello following values, and then click **Next**:</span></span>
   * <span data-ttu-id="ba70d-151">Ange ett namn för hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="ba70d-151">Enter a name for hello project.</span></span>
   * <span data-ttu-id="ba70d-152">I hello **JRE** område, se till att **använder en körningsmiljö JRE** har angetts för**JavaSE 1.7** eller senare.</span><span class="sxs-lookup"><span data-stu-id="ba70d-152">In hello **JRE** area, make sure that **Use an execution environment JRE** is set too**JavaSE-1.7** or later.</span></span>
   * <span data-ttu-id="ba70d-153">Se till att Spark SDK är toohello platsen dit du hämtade hello SDK.</span><span class="sxs-lookup"><span data-stu-id="ba70d-153">Make sure that Spark SDK is set toohello location where you downloaded hello SDK.</span></span> <span data-ttu-id="ba70d-154">hello länken toohello hämtningsplats ingår i hello [krav](#prerequisites) tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="ba70d-154">hello link toohello download location is included in hello [prerequisites](#prerequisites) earlier in this article.</span></span> <span data-ttu-id="ba70d-155">Du kan också hämta hello SDK från hello länk som ingår i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ba70d-155">You can also download hello SDK from hello link included in hello dialog box.</span></span>

    ![Dialogrutan Nytt projekt för HDInsight Scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
5.  <span data-ttu-id="ba70d-157">Klicka på hello hello nästa i dialogrutan **bibliotek** fliken hålla hello standardvärden och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-157">In hello next dialog box, click hello **Libraries** tab and keep hello defaults, and then click **Finish**.</span></span> 
   
    ![Biblioteksfliken](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a><span data-ttu-id="ba70d-159">Skapa ett Scala program för ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="ba70d-159">Create a Scala application for an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="ba70d-160">Expandera hello-projekt som du skapade tidigare i hello Eclipse IDE från paketet Explorer, högerklicka på **src**, peka för**ny**, och klicka sedan på **andra**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-160">In hello Eclipse IDE, from Package Explorer, expand hello project that you created earlier, right-click **src**, point too**New**, and then click **Other**.</span></span>
2. <span data-ttu-id="ba70d-161">I hello **Välj en guide** dialogrutan Expandera **Scala guider**, klickar du på **Scala objekt**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-161">In hello **Select a wizard** dialog box, expand **Scala Wizards**, click **Scala Object**, and then click **Next**.</span></span>
   
    ![Välj en dialogruta för guiden](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. <span data-ttu-id="ba70d-163">I hello **Skapa ny fil** dialogrutan Ange ett namn för hello-objektet och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-163">In hello **Create New File** dialog box, enter a name for hello object, and then click **Finish**.</span></span>
   
    ![Skapa en ny fil dialogruta](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. <span data-ttu-id="ba70d-165">Klistra in följande kod i hello textredigerare hello:</span><span class="sxs-lookup"><span data-stu-id="ba70d-165">Paste hello following code in hello text editor:</span></span>
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        object MyClusterApp{
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find hello rows that have only one digit in hello seventh column in hello CSV
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACOut")
          }        
        }
5. <span data-ttu-id="ba70d-166">Kör hello på ett HDInsight Spark-kluster:</span><span class="sxs-lookup"><span data-stu-id="ba70d-166">Run hello application on an HDInsight Spark cluster:</span></span>
   
   1. <span data-ttu-id="ba70d-167">Högerklicka på projektnamnet hello från Utforskaren i paketet, och välj sedan **skicka Spark-program tooHDInsight**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-167">From Package Explorer, right-click hello project name, and then select **Submit Spark Application tooHDInsight**.</span></span>        
   2. <span data-ttu-id="ba70d-168">I hello **Spark skicka** dialogrutan hello följande värden, och klickar sedan på **skicka**:</span><span class="sxs-lookup"><span data-stu-id="ba70d-168">In hello **Spark Submission** dialog box, provide hello following values, and then click **Submit**:</span></span>
      
      * <span data-ttu-id="ba70d-169">För **klusternamnet**, Välj hello HDInsight Spark-kluster som du vill använda toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="ba70d-169">For **Cluster Name**, select hello HDInsight Spark cluster on which you want toorun your application.</span></span>
      * <span data-ttu-id="ba70d-170">Välj en artefakt hello Eclipse-projektet, eller välja en från en hårddisk.</span><span class="sxs-lookup"><span data-stu-id="ba70d-170">Select an artifact from hello Eclipse project, or select one from a hard drive.</span></span> <span data-ttu-id="ba70d-171">hello Standardvärdet beror på hello objekt du högerklickar på paketet Explorer.</span><span class="sxs-lookup"><span data-stu-id="ba70d-171">hello default value depends on hello item you right-click from package explorer.</span></span>
      * <span data-ttu-id="ba70d-172">I hello **Main klassnamn** dropdownlist skickas visas alla objektnamn från projektet valda.</span><span class="sxs-lookup"><span data-stu-id="ba70d-172">In hello **Main class name** dropdownlist, submission wizard displays all object names from your selected project.</span></span> <span data-ttu-id="ba70d-173">Välj eller ange en som du vill toorun.</span><span class="sxs-lookup"><span data-stu-id="ba70d-173">Select or input one that you want toorun.</span></span> <span data-ttu-id="ba70d-174">Om du väljer artefakt från hårddisk måste du ange huvudsakliga klassnamn själv.</span><span class="sxs-lookup"><span data-stu-id="ba70d-174">If you select artifact from hard disk, you need input main class name by yourself.</span></span> 
      * <span data-ttu-id="ba70d-175">Eftersom hello programkod i det här exemplet inte kräver några kommandoradsargument eller referera burkar eller filer, kan du lämna hello återstående textrutor är tom.</span><span class="sxs-lookup"><span data-stu-id="ba70d-175">Because hello application code in this example does not require any command-line arguments or reference JARs or files, you can leave hello remaining text boxes empty.</span></span>
        
       ![Dialogrutan Skicka Spark](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
   3. <span data-ttu-id="ba70d-177">Hej **Spark skicka** fliken ska börja visas hello pågår.</span><span class="sxs-lookup"><span data-stu-id="ba70d-177">hello **Spark Submission** tab should start displaying hello progress.</span></span> <span data-ttu-id="ba70d-178">Du kan stoppa hello program genom att klicka hello red i hello **Spark skicka** fönster.</span><span class="sxs-lookup"><span data-stu-id="ba70d-178">You can stop hello application by clicking hello red button in hello **Spark Submission** window.</span></span> <span data-ttu-id="ba70d-179">Du kan också visa hello loggar för specifika programmet kör genom att klicka på hello Globikon (betecknas med hello blå ruta hello bild).</span><span class="sxs-lookup"><span data-stu-id="ba70d-179">You can also view hello logs for this specific application run by clicking hello globe icon (denoted by hello blue box in hello image).</span></span>
      
       ![Skicka Spark-fönster](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a><span data-ttu-id="ba70d-181">Komma åt och hantera HDInsight Spark-kluster med hjälp av HDInsight-verktyg i Azure Toolkit för Eclipse</span><span class="sxs-lookup"><span data-stu-id="ba70d-181">Access and manage HDInsight Spark clusters by using HDInsight Tools in Azure Toolkit for Eclipse</span></span>
<span data-ttu-id="ba70d-182">Du kan utföra olika åtgärder med hjälp av HDInsight-verktyg, inklusive åtkomst till hello jobbutdata.</span><span class="sxs-lookup"><span data-stu-id="ba70d-182">You can perform various operations by using HDInsight Tools, including accessing hello job output.</span></span>

### <a name="access-hello-job-view"></a><span data-ttu-id="ba70d-183">Åtkomst hello jobbet vy</span><span class="sxs-lookup"><span data-stu-id="ba70d-183">Access hello job view</span></span>
1. <span data-ttu-id="ba70d-184">I Azure Explorer expanderar **HDInsight**, expandera klusternamnet i hello Spark och klicka sedan på **jobb**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-184">In Azure Explorer, expand **HDInsight**, expand hello Spark cluster name, and then click **Jobs**.</span></span> 

    ![Jobbet visa nod](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. <span data-ttu-id="ba70d-186">Klicka på hello **jobb** nod.</span><span class="sxs-lookup"><span data-stu-id="ba70d-186">Click on hello **Jobs** node.</span></span> <span data-ttu-id="ba70d-187">Hej HDInsight Tools identifieras om du har installerat hello E (fx) clipse plugin-programmet eller inte.</span><span class="sxs-lookup"><span data-stu-id="ba70d-187">hello HDInsight Tools auto-detects whether you installed hello E(fx)clipse plugin or not.</span></span> <span data-ttu-id="ba70d-188">Klicka på **OK** toocontinue och följ hello instruktioner tooinstall hello Eclipse Marketplace och starta om Eclipse.</span><span class="sxs-lookup"><span data-stu-id="ba70d-188">Click **OK** toocontinue and follow hello instructions tooinstall hello Eclipse Marketplace and restart Eclipse.</span></span>

    ![Installera clipse E (fx)](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

3. <span data-ttu-id="ba70d-190">Öppna hello jobbet från hello **jobb** nod.</span><span class="sxs-lookup"><span data-stu-id="ba70d-190">Open hello Job View from hello **Jobs** node.</span></span> <span data-ttu-id="ba70d-191">I högra fönstret hello hello **Spark jobbet vyn** visar alla hello-program som kördes på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="ba70d-191">In hello right pane, hello **Spark Job View** tab displays all hello applications that were run on hello cluster.</span></span> <span data-ttu-id="ba70d-192">Klicka på hello namnet på hello program som du vill toosee mer information.</span><span class="sxs-lookup"><span data-stu-id="ba70d-192">Click hello name of hello application for which you want toosee more details.</span></span>

    ![Programinformation](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)
4. <span data-ttu-id="ba70d-194">Om du hovrar på hello jobbdiagram visar grundläggande körs Jobbinformationen.</span><span class="sxs-lookup"><span data-stu-id="ba70d-194">If you hover on hello job graph, it displays basic running job info.</span></span> <span data-ttu-id="ba70d-195">Visas klickar på hello jobbdiagram hello steg diagram och information som genereras av varje jobb.</span><span class="sxs-lookup"><span data-stu-id="ba70d-195">Clicking on hello job graph shows hello stages graph and info that every job generates.</span></span>

    ![Steget jobbinformation](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

5. <span data-ttu-id="ba70d-197">Vanliga loggar, inklusive drivrutinen Stderr, drivrutinen Stdout och Directory information visas i hello **loggen** fliken.</span><span class="sxs-lookup"><span data-stu-id="ba70d-197">Frequently used logs, including Driver Stderr, Driver Stdout, and Directory Info, are listed in hello **Log** tab.</span></span>

    ![Logginformation](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)
6. <span data-ttu-id="ba70d-199">Du kan också öppna hello Spark historik UI och hello YARN-Användargränssnittet (på programnivå hello) genom att klicka på hello respektive hyperlink hello överst i fönstret hello.</span><span class="sxs-lookup"><span data-stu-id="ba70d-199">You can also open hello Spark history UI and hello YARN UI (at hello application level) by clicking hello respective hyperlink at hello top of hello window.</span></span>

### <a name="access-hello-storage-container-for-hello-cluster"></a><span data-ttu-id="ba70d-200">Åtkomst hello lagringsbehållare som klustret hello</span><span class="sxs-lookup"><span data-stu-id="ba70d-200">Access hello storage container for hello cluster</span></span>
1. <span data-ttu-id="ba70d-201">I Azure Explorer expanderar du hello **HDInsight** rot nod toosee en lista över HDInsight Spark-kluster som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="ba70d-201">In Azure Explorer, expand hello **HDInsight** root node toosee a list of HDInsight Spark clusters that are available.</span></span>
2. <span data-ttu-id="ba70d-202">Expandera hello toosee hello lagring klusternamnkontot och hello standardbehållare för lagring för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="ba70d-202">Expand hello cluster name toosee hello storage account and hello default storage container for hello cluster.</span></span>
   
    ![Storage-konto och standard lagringsbehållare](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. <span data-ttu-id="ba70d-204">Klicka på hello namnet på lagringsbehållaren som associerats med hello kluster.</span><span class="sxs-lookup"><span data-stu-id="ba70d-204">Click hello storage container name associated with hello cluster.</span></span> <span data-ttu-id="ba70d-205">I hello högra rutan, dubbelklickar du på hello **HVACOut** mapp.</span><span class="sxs-lookup"><span data-stu-id="ba70d-205">In hello right pane, double-click hello **HVACOut** folder.</span></span> <span data-ttu-id="ba70d-206">Öppna en hello **del -** toosee hello utdata från programmet hello-filer.</span><span class="sxs-lookup"><span data-stu-id="ba70d-206">Open one of hello **part-** files toosee hello output of hello application.</span></span>

### <a name="access-hello-spark-history-server"></a><span data-ttu-id="ba70d-207">Historik fjärråtkomstserver hello Spark</span><span class="sxs-lookup"><span data-stu-id="ba70d-207">Access hello Spark history server</span></span>
1. <span data-ttu-id="ba70d-208">Högerklicka på klusternamnet Spark i Azure Explorer och markera **öppna Spark historik UI**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-208">In Azure Explorer, right-click your Spark cluster name, and then select **Open Spark History UI**.</span></span> <span data-ttu-id="ba70d-209">När du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="ba70d-209">When you're prompted, enter hello admin credentials for hello cluster.</span></span> <span data-ttu-id="ba70d-210">Du måste ange dessa vid etablering hello klustret.</span><span class="sxs-lookup"><span data-stu-id="ba70d-210">You must have specified these while provisioning hello cluster.</span></span>
2. <span data-ttu-id="ba70d-211">I instrumentpanelen för hello Spark historik server använder du hello programmet namnet toolook för hello program just avslutats körs.</span><span class="sxs-lookup"><span data-stu-id="ba70d-211">In hello Spark history server dashboard, you use hello application name toolook for hello application that you just finished running.</span></span> <span data-ttu-id="ba70d-212">I hello föregående kod, ange hello programnamn med `val conf = new SparkConf().setAppName("MyClusterApp")`.</span><span class="sxs-lookup"><span data-stu-id="ba70d-212">In hello preceding code, you set hello application name by using `val conf = new SparkConf().setAppName("MyClusterApp")`.</span></span> <span data-ttu-id="ba70d-213">Därför programnamnet Spark har **MyClusterApp**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-213">Hence, your Spark application name was **MyClusterApp**.</span></span>

### <a name="start-hello-ambari-portal"></a><span data-ttu-id="ba70d-214">Starta hello Ambari-portalen</span><span class="sxs-lookup"><span data-stu-id="ba70d-214">Start hello Ambari portal</span></span>
1. <span data-ttu-id="ba70d-215">Högerklicka på klusternamnet Spark i Azure Explorer och markera **öppna klustret hanteringsportalen (Ambari)**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-215">In Azure Explorer, right-click your Spark cluster name, and then select **Open Cluster Management Portal (Ambari)**.</span></span> 
2. <span data-ttu-id="ba70d-216">När du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="ba70d-216">When you're prompted, enter hello admin credentials for hello cluster.</span></span> <span data-ttu-id="ba70d-217">Du måste ange dessa vid etablering hello klustret.</span><span class="sxs-lookup"><span data-stu-id="ba70d-217">You must have specified these while provisioning hello cluster.</span></span>

### <a name="manage-azure-subscriptions"></a><span data-ttu-id="ba70d-218">Hantera Azure-prenumerationer</span><span class="sxs-lookup"><span data-stu-id="ba70d-218">Manage Azure subscriptions</span></span>
<span data-ttu-id="ba70d-219">Som standard visar HDInsight-verktyg i Azure Toolkit för Eclipse hello Spark-kluster från alla dina Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="ba70d-219">By default, HDInsight Tools in Azure Toolkit for Eclipse lists hello Spark clusters from all your Azure subscriptions.</span></span> <span data-ttu-id="ba70d-220">Du kan ange hello prenumerationer som du vill tooaccess hello klustret om det behövs.</span><span class="sxs-lookup"><span data-stu-id="ba70d-220">If necessary, you can specify hello subscriptions for which you want tooaccess hello cluster.</span></span> 

1. <span data-ttu-id="ba70d-221">Högerklicka i Azure Explorer hello **Azure** rot noden och klicka sedan på **hantera prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-221">In Azure Explorer, right-click hello **Azure** root node, and then click **Manage Subscriptions**.</span></span> 
2. <span data-ttu-id="ba70d-222">I dialogrutan för hello avmarkera hello för hello prenumeration som du inte vill tooaccess och klicka sedan på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-222">In hello dialog box, clear hello check boxes for hello subscription that you don't want tooaccess, and then click **Close**.</span></span> <span data-ttu-id="ba70d-223">Du kan också klicka på **logga ut** om du vill toosign utanför din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ba70d-223">You can also click **Sign Out** if you want toosign out of your Azure subscription.</span></span>

## <a name="run-a-spark-scala-application-locally"></a><span data-ttu-id="ba70d-224">Köra ett program med Spark Scala lokalt</span><span class="sxs-lookup"><span data-stu-id="ba70d-224">Run a Spark Scala application locally</span></span>
<span data-ttu-id="ba70d-225">Du kan använda HDInsight Tools i Azure Toolkit för Eclipse toorun Spark Scala program lokalt på din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="ba70d-225">You can use HDInsight Tools in Azure Toolkit for Eclipse toorun Spark Scala applications locally on your workstation.</span></span> <span data-ttu-id="ba70d-226">Normalt programmen behöver inte komma åt toocluster resurser, till exempel en lagringsbehållare och du kan köra och testa dem lokalt.</span><span class="sxs-lookup"><span data-stu-id="ba70d-226">Typically, these applications don't need access toocluster resources such as a storage container, and you can run and test them locally.</span></span>

### <a name="prerequisite"></a><span data-ttu-id="ba70d-227">Krav</span><span class="sxs-lookup"><span data-stu-id="ba70d-227">Prerequisite</span></span>
<span data-ttu-id="ba70d-228">När du kör hello lokalt Spark Scala-program på en Windows-dator, kan du få ett undantag som beskrivs i [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356).</span><span class="sxs-lookup"><span data-stu-id="ba70d-228">While you're running hello local Spark Scala application on a Windows computer, you might get an exception as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span></span> <span data-ttu-id="ba70d-229">Det här undantaget inträffar eftersom **WinUtils.exe** saknas i Windows.</span><span class="sxs-lookup"><span data-stu-id="ba70d-229">This exception occurs because **WinUtils.exe** is missing in Windows.</span></span> 

<span data-ttu-id="ba70d-230">tooresolve det här felet måste du [hämta hello körbara](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa plats som **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-230">tooresolve this error, you must [download hello executable](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa location like **C:\WinUtils\bin**.</span></span> <span data-ttu-id="ba70d-231">Sedan måste du lägga till hello miljövariabeln **HADOOP_HOME** och ange hello hello variabels värde för**C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-231">You must then add hello environment variable **HADOOP_HOME** and set hello value of hello variable too**C\WinUtils**.</span></span>

### <a name="run-a-local-spark-scala-application"></a><span data-ttu-id="ba70d-232">Kör ett lokalt Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="ba70d-232">Run a local Spark Scala application</span></span>
1. <span data-ttu-id="ba70d-233">Starta Eclipse och skapa ett projekt.</span><span class="sxs-lookup"><span data-stu-id="ba70d-233">Start Eclipse and create a project.</span></span> <span data-ttu-id="ba70d-234">I hello **nytt projekt** dialogrutan att Hej följande alternativ och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-234">In hello **New Project** dialog box, make hello following choices, and then click **Next**.</span></span>
   
   * <span data-ttu-id="ba70d-235">I hello vänster och välj **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-235">In hello left pane, select **HDInsight**.</span></span>
   * <span data-ttu-id="ba70d-236">I hello högra rutan, Välj **Spark på HDInsight lokala kör sampel (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-236">In hello right pane, select **Spark on HDInsight Local Run Sample (Scala)**.</span></span>

    ![Dialogrutan Nytt projekt](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
2. <span data-ttu-id="ba70d-238">tooprovide hello projektinformation, Följ steg 3 till 6 från hello tidigare avsnittet [Ställ in ett Spark Scala-projekt för ett HDInsight Spark-kluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).</span><span class="sxs-lookup"><span data-stu-id="ba70d-238">tooprovide hello project details, follow steps 3 through 6 from hello earlier section [Set up a Spark Scala project for an HDInsight Spark cluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).</span></span>
3. <span data-ttu-id="ba70d-239">hello mallen lägger till en exempelkod (**LogQuery**) under hello **src** mapp som du kan köra lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="ba70d-239">hello template adds a sample code (**LogQuery**) under hello **src** folder that you can run locally on your computer.</span></span>
   
    ![Platsen för LogQuery](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)
4. <span data-ttu-id="ba70d-241">Högerklicka på hello **LogQuery** program, peka för**kör som**, och klicka sedan på **1 Scala program**.</span><span class="sxs-lookup"><span data-stu-id="ba70d-241">Right-click hello **LogQuery** application, point too**Run As**, and then click **1 Scala Application**.</span></span> <span data-ttu-id="ba70d-242">Du kommer att se utdata som liknar detta i hello **konsolen** fliken längst ned hello:</span><span class="sxs-lookup"><span data-stu-id="ba70d-242">You will see an output like this in hello **Console** tab at hello bottom:</span></span>
   
   ![Spark programmet lokala kör resultat](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="faq"></a><span data-ttu-id="ba70d-244">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="ba70d-244">FAQ</span></span>
<span data-ttu-id="ba70d-245">toosubmit ett program tooAzure Data Lake Store, Välj **interaktiv** läge under hello Azure-inloggning.</span><span class="sxs-lookup"><span data-stu-id="ba70d-245">toosubmit an application tooAzure Data Lake Store, choose **Interactive** mode during hello Azure sign-in process.</span></span> <span data-ttu-id="ba70d-246">Om du väljer **automatisk** läge, du får ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="ba70d-246">If you select **Automated** mode, you can get an error.</span></span>

![Interaktiv inloggning](./media/hdinsight-apache-spark-eclipse-tool-plugin/interactive-authentication.png)

<span data-ttu-id="ba70d-248">Nu ska löste vi problemet.</span><span class="sxs-lookup"><span data-stu-id="ba70d-248">Now, we resolved it.</span></span> <span data-ttu-id="ba70d-249">Du kan välja ett Azure Data Lake klustret toosubmit ditt program med valfri metod för inloggning.</span><span class="sxs-lookup"><span data-stu-id="ba70d-249">You can choose an Azure Data Lake Cluster toosubmit your application with any sign-in method.</span></span>

## <a name="feedback-and-known-issues"></a><span data-ttu-id="ba70d-250">Feedback och kända problem</span><span class="sxs-lookup"><span data-stu-id="ba70d-250">Feedback and known issues</span></span>
<span data-ttu-id="ba70d-251">För närvarande kan stöds visa Spark utdata direkt inte.</span><span class="sxs-lookup"><span data-stu-id="ba70d-251">Currently, viewing Spark outputs directly is not supported.</span></span>

<span data-ttu-id="ba70d-252">Om du har förslag eller feedback eller om du stöter på problem när du använder det här verktyget känna sig fria toosend oss ett e-postmeddelande på hdivstool@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="ba70d-252">If you have any suggestions or feedback, or if you encounter any problems when using this tool, feel free toosend us an email at hdivstool@microsoft.com.</span></span>

## <span data-ttu-id="ba70d-253"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="ba70d-253"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="ba70d-254">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="ba70d-254">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="ba70d-255">Scenarier</span><span class="sxs-lookup"><span data-stu-id="ba70d-255">Scenarios</span></span>
* [<span data-ttu-id="ba70d-256">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="ba70d-256">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="ba70d-257">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="ba70d-257">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="ba70d-258">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="ba70d-258">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="ba70d-259">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="ba70d-259">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="ba70d-260">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ba70d-260">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a><span data-ttu-id="ba70d-261">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="ba70d-261">Creating and running applications</span></span>
* [<span data-ttu-id="ba70d-262">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="ba70d-262">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="ba70d-263">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="ba70d-263">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="ba70d-264">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="ba70d-264">Tools and extensions</span></span>
* [<span data-ttu-id="ba70d-265">Använd Azure Toolkit för IntelliJ toocreate och skicka Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="ba70d-265">Use Azure Toolkit for IntelliJ toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="ba70d-266">Använd Azure Toolkit för IntelliJ toodebug Spark-program via fjärranslutning via VPN</span><span class="sxs-lookup"><span data-stu-id="ba70d-266">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="ba70d-267">Använd Azure Toolkit för IntelliJ toodebug Spark-program via fjärranslutning via SSH</span><span class="sxs-lookup"><span data-stu-id="ba70d-267">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="ba70d-268">Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="ba70d-268">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="ba70d-269">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ba70d-269">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="ba70d-270">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="ba70d-270">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="ba70d-271">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="ba70d-271">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="ba70d-272">Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="ba70d-272">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a><span data-ttu-id="ba70d-273">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="ba70d-273">Managing resources</span></span>
* [<span data-ttu-id="ba70d-274">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="ba70d-274">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="ba70d-275">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ba70d-275">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

