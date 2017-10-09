---
title: "aaaUse Zeppelin-anteckningsböcker med Apache Spark-kluster i Azure HDInsight | Microsoft Docs"
description: "Stegvisa instruktioner för hur toouse Zeppelin-anteckningsböcker med Apache Spark-kluster i Azure HDInsight."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: df489d70-7788-4efa-a089-e5e5006421e2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 3ab479cfccc7fd38a9bf6a9fb4f5928beec8ff7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a><span data-ttu-id="1081c-103">Använda Zeppelin-anteckningsböcker med Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="1081c-103">Use Zeppelin notebooks with Apache Spark cluster on Azure HDInsight</span></span>

<span data-ttu-id="1081c-104">HDInsight Spark-kluster innehåller Zeppelin-anteckningsböcker som du kan använda toorun Spark jobb.</span><span class="sxs-lookup"><span data-stu-id="1081c-104">HDInsight Spark clusters include Zeppelin notebooks that you can use toorun Spark jobs.</span></span> <span data-ttu-id="1081c-105">I den här artikeln lär du dig hur toouse hello Zeppelin anteckningsbok på ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1081c-105">In this article, you learn how toouse hello Zeppelin notebook on an HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="1081c-106">Zeppelin-anteckningsböcker är endast tillgängligt för Spark 1.6.3 i HDInsight 3.5 och Spark 2.1.0 i HDInsight 3,6.</span><span class="sxs-lookup"><span data-stu-id="1081c-106">Zeppelin notebooks are available only for Spark 1.6.3 on HDInsight 3.5 and Spark 2.1.0 on HDInsight 3.6.</span></span>
>

<span data-ttu-id="1081c-107">**Krav:**</span><span class="sxs-lookup"><span data-stu-id="1081c-107">**Prerequisites:**</span></span>

* <span data-ttu-id="1081c-108">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1081c-108">An Azure subscription.</span></span> <span data-ttu-id="1081c-109">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="1081c-109">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="1081c-110">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1081c-110">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="1081c-111">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="1081c-111">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="launch-a-zeppelin-notebook"></a><span data-ttu-id="1081c-112">Starta en Zeppelin-anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="1081c-112">Launch a Zeppelin notebook</span></span>
1. <span data-ttu-id="1081c-113">Hello Spark-klusterbladet, klicka på **Klusterinstrumentpanel**, och klicka sedan på **Zeppelin anteckningsboken**.</span><span class="sxs-lookup"><span data-stu-id="1081c-113">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Zeppelin Notebook**.</span></span> <span data-ttu-id="1081c-114">Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="1081c-114">If prompted, enter hello admin credentials for hello cluster.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1081c-115">Du kan också nå hello Zeppelin anteckningsboken för klustret genom att öppna hello följande URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="1081c-115">You may also reach hello Zeppelin Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="1081c-116">Ersätt **KLUSTERNAMN** med hello namnet på klustret:</span><span class="sxs-lookup"><span data-stu-id="1081c-116">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`
   > 
   > 
2. <span data-ttu-id="1081c-117">Skapa en ny anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="1081c-117">Create a new notebook.</span></span> <span data-ttu-id="1081c-118">Hej rubrikfönstret klickar du på **anteckningsboken**, och klicka sedan på **Skapa ny anteckning**.</span><span class="sxs-lookup"><span data-stu-id="1081c-118">From hello header pane, click **Notebook**, and then click **Create New Note**.</span></span>
   
    <span data-ttu-id="1081c-119">![Skapa en ny anteckningsbok Zeppelin](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "skapa en ny anteckningsbok Zeppelin")</span><span class="sxs-lookup"><span data-stu-id="1081c-119">![Create a new Zeppelin notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "Create a new Zeppelin notebook")</span></span>
   
    <span data-ttu-id="1081c-120">Ange ett namn för anteckningsboken hello och klicka sedan på **skapa anteckning**.</span><span class="sxs-lookup"><span data-stu-id="1081c-120">Enter a name for hello notebook, and then click **Create Note**.</span></span>
3. <span data-ttu-id="1081c-121">Kontrollera också att hello anteckningsboken sidhuvud visar status för anslutna.</span><span class="sxs-lookup"><span data-stu-id="1081c-121">Also, make sure hello notebook header shows a connected status.</span></span> <span data-ttu-id="1081c-122">Den betecknas med en grön punkt i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="1081c-122">It is denoted by a green dot in hello top-right corner.</span></span>
   
    <span data-ttu-id="1081c-123">![Status för Zeppelin-anteckningsboken](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin anteckningsboken status")</span><span class="sxs-lookup"><span data-stu-id="1081c-123">![Zeppelin notebook status](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin notebook status")</span></span>
4. <span data-ttu-id="1081c-124">Läs in exempeldata i en tillfällig tabell.</span><span class="sxs-lookup"><span data-stu-id="1081c-124">Load sample data into a temporary table.</span></span> <span data-ttu-id="1081c-125">När du skapar ett Spark-kluster i HDInsight, hello exempeldatafil **hvac.csv**, är kopierade toohello associerade lagringskontot under **\HdiSamples\SensorSampleData\hvac**.</span><span class="sxs-lookup"><span data-stu-id="1081c-125">When you create a Spark cluster in HDInsight, hello sample data file, **hvac.csv**, is copied toohello associated storage account under **\HdiSamples\SensorSampleData\hvac**.</span></span>
   
    <span data-ttu-id="1081c-126">Klistra in följande kodutdrag hello i hello tomt stycke som skapas som standard i hello ny anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="1081c-126">In hello empty paragraph that is created by default in hello new notebook, paste hello following snippet.</span></span>
   
        %livy.spark
        //hello above magic instructs Zeppelin toouse hello Livy Scala interpreter
   
        // Create an RDD using hello default Spark context, sc
        val hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
   
        // Map hello values in hello .csv file toohello schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
   
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
   
    <span data-ttu-id="1081c-127">Tryck på **SKIFT + RETUR** eller klicka på hello **spela upp** knapp för hello punkt toorun hello fragment.</span><span class="sxs-lookup"><span data-stu-id="1081c-127">Press **SHIFT + ENTER** or click hello **Play** button for hello paragraph toorun hello snippet.</span></span> <span data-ttu-id="1081c-128">hello status på hello högra hörnet av hello punkt ska passera från klar, VÄNTANDE tooFINISHED KÖRS.</span><span class="sxs-lookup"><span data-stu-id="1081c-128">hello status on hello right-corner of hello paragraph should progress from READY, PENDING, RUNNING tooFINISHED.</span></span> <span data-ttu-id="1081c-129">hello utdata visas längst ned hello hello samma punkt.</span><span class="sxs-lookup"><span data-stu-id="1081c-129">hello output shows up at hello bottom of hello same paragraph.</span></span> <span data-ttu-id="1081c-130">Det ser ut så hello följande hello skärmbild:</span><span class="sxs-lookup"><span data-stu-id="1081c-130">hello screenshot looks like hello following:</span></span>
   
    <span data-ttu-id="1081c-131">![Skapa en tillfällig tabell från rådata](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "skapa en tillfällig tabell från rådata")</span><span class="sxs-lookup"><span data-stu-id="1081c-131">![Create a temporary table from raw data](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "Create a temporary table from raw data")</span></span>
   
    <span data-ttu-id="1081c-132">Du kan också ange en avdelning tooeach punkt.</span><span class="sxs-lookup"><span data-stu-id="1081c-132">You can also provide a title tooeach paragraph.</span></span> <span data-ttu-id="1081c-133">Hello högra hörnet och klicka på hello **inställningar** ikonen och klickar sedan på **Visa rubrik**.</span><span class="sxs-lookup"><span data-stu-id="1081c-133">From hello right-hand corner, click hello **Settings** icon, and then click **Show title**.</span></span>
5. <span data-ttu-id="1081c-134">Du kan nu köra Spark SQL-uttryck på hello **hvac** tabell.</span><span class="sxs-lookup"><span data-stu-id="1081c-134">You can now run Spark SQL statements on hello **hvac** table.</span></span> <span data-ttu-id="1081c-135">Klistra in följande fråga i ett nytt stycke hello.</span><span class="sxs-lookup"><span data-stu-id="1081c-135">Paste hello following query in a new paragraph.</span></span> <span data-ttu-id="1081c-136">hello fråga hämtar hello byggnad ID och hello skillnaden mellan hello mål och faktiska temperaturer för varje bygger på ett visst datum.</span><span class="sxs-lookup"><span data-stu-id="1081c-136">hello query retrieves hello building ID and hello difference between hello target and actual temperatures for each building on a given date.</span></span> <span data-ttu-id="1081c-137">Tryck på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="1081c-137">Press **SHIFT + ENTER**.</span></span>
   
        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
   
    <span data-ttu-id="1081c-138">Hej **% sql** instruktionen hello början visar hello anteckningsboken toouse hello Livius Scala tolken.</span><span class="sxs-lookup"><span data-stu-id="1081c-138">hello **%sql** statement at hello beginning tells hello notebook toouse hello Livy Scala interpreter.</span></span>
   
    <span data-ttu-id="1081c-139">hello visar följande skärmbild hello utdata.</span><span class="sxs-lookup"><span data-stu-id="1081c-139">hello following screenshot shows hello output.</span></span>
   
    <span data-ttu-id="1081c-140">![Köra Spark SQL-uttryck med hello anteckningsboken](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "köra Spark SQL-uttryck med hello anteckningsboken")</span><span class="sxs-lookup"><span data-stu-id="1081c-140">![Run a Spark SQL statement using hello notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "Run a Spark SQL statement using hello notebook")</span></span>
   
     <span data-ttu-id="1081c-141">Klicka på hello Visa alternativ (markerade i rektangel) tooswitch mellan olika representationer för hello samma utdata.</span><span class="sxs-lookup"><span data-stu-id="1081c-141">Click hello display options (highlighted in rectangle) tooswitch between different representations for hello same output.</span></span> <span data-ttu-id="1081c-142">Klicka på **inställningar** toochoose vilka consitutes hello nyckel och värden i hello utdata.</span><span class="sxs-lookup"><span data-stu-id="1081c-142">Click **Settings** toochoose what consitutes hello key and values in hello output.</span></span> <span data-ttu-id="1081c-143">Hej skärmdump ovan använder **buildingID** som hello nyckel och hello medelvärdet av **temp_diff** som hello-värde.</span><span class="sxs-lookup"><span data-stu-id="1081c-143">hello screen capture above uses **buildingID** as hello key and hello average of **temp_diff** as hello value.</span></span>
6. <span data-ttu-id="1081c-144">Du kan också köra Spark SQL-uttryck med hjälp av variabler i hello-frågan.</span><span class="sxs-lookup"><span data-stu-id="1081c-144">You can also run Spark SQL statements using variables in hello query.</span></span> <span data-ttu-id="1081c-145">Hej nästa fragment visar hur toodefine en variabel, **Temp**, i hello frågan med hello möjliga värden du tooquery med.</span><span class="sxs-lookup"><span data-stu-id="1081c-145">hello next snippet shows how toodefine a variable, **Temp**, in hello query with hello possible values you want tooquery with.</span></span> <span data-ttu-id="1081c-146">När du först kör hello frågan fylls automatiskt en listruta med hello-värden som du har angett för hello-variabeln.</span><span class="sxs-lookup"><span data-stu-id="1081c-146">When you first run hello query, a drop-down is automatically populated with hello values you specified for hello variable.</span></span>
   
        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 
   
    <span data-ttu-id="1081c-147">Klistra in följande kodutdrag i en ny punkt och trycker på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="1081c-147">Paste this snippet in a new paragraph and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="1081c-148">hello visar följande skärmbild hello utdata.</span><span class="sxs-lookup"><span data-stu-id="1081c-148">hello following screenshot shows hello output.</span></span>
   
    <span data-ttu-id="1081c-149">![Köra Spark SQL-uttryck med hello anteckningsboken](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "köra Spark SQL-uttryck med hello anteckningsboken")</span><span class="sxs-lookup"><span data-stu-id="1081c-149">![Run a Spark SQL statement using hello notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "Run a Spark SQL statement using hello notebook")</span></span>
   
    <span data-ttu-id="1081c-150">För efterföljande frågor kan du välja ett nytt värde hello listrutan och kör hello frågan igen.</span><span class="sxs-lookup"><span data-stu-id="1081c-150">For subsequent queries, you can select a new value from hello drop-down and run hello query again.</span></span> <span data-ttu-id="1081c-151">Klicka på **inställningar** toochoose vilka consitutes hello nyckel och värden i hello utdata.</span><span class="sxs-lookup"><span data-stu-id="1081c-151">Click **Settings** toochoose what consitutes hello key and values in hello output.</span></span> <span data-ttu-id="1081c-152">Hej skärmdump ovan använder **buildingID** som hello nyckel hello medelvärde för **temp_diff** som hello värde och **targettemp** som hello grupp.</span><span class="sxs-lookup"><span data-stu-id="1081c-152">hello screen capture above uses **buildingID** as hello key, hello average of **temp_diff** as hello value, and **targettemp** as hello group.</span></span>
7. <span data-ttu-id="1081c-153">Starta om hello Livius tolken tooexit hello program.</span><span class="sxs-lookup"><span data-stu-id="1081c-153">Restart hello Livy interpreter tooexit hello application.</span></span> <span data-ttu-id="1081c-154">toodo det, öppnar tolkens inställningar genom att klicka på hello inloggad användarnamnet från hello övre högra hörnet och klicka sedan på **tolken**.</span><span class="sxs-lookup"><span data-stu-id="1081c-154">toodo so, open interpreter settings by clicking hello logged in user name from hello top-right corner, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="1081c-155">![Starta tolken](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive utdata")</span><span class="sxs-lookup"><span data-stu-id="1081c-155">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
8. <span data-ttu-id="1081c-156">Rulla tooLivy tolkens inställningar och klickar sedan på **starta om**.</span><span class="sxs-lookup"><span data-stu-id="1081c-156">Scroll tooLivy interpreter settings and then click **Restart**.</span></span>
   
    <span data-ttu-id="1081c-157">![Starta om hello Livius intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "starta om hello Zeppelin intepreter")</span><span class="sxs-lookup"><span data-stu-id="1081c-157">![Restart hello Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Restart hello Zeppelin intepreter")</span></span>

## <a name="how-do-i-use-external-packages-with-hello-notebook"></a><span data-ttu-id="1081c-158">Hur använder externa paket med hello bärbara?</span><span class="sxs-lookup"><span data-stu-id="1081c-158">How do I use external packages with hello notebook?</span></span>
<span data-ttu-id="1081c-159">Du kan konfigurera hello Zeppelin anteckningsboken i Apache Spark-kluster i HDInsight (Linux) toouse externa, community-bidragit paket som inte är inkluderade out-of-the-box i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="1081c-159">You can configure hello Zeppelin notebook in Apache Spark cluster on HDInsight (Linux) toouse external, community-contributed packages that are not included out-of-the-box in hello cluster.</span></span> <span data-ttu-id="1081c-160">Du kan söka hello [Maven databasen](http://search.maven.org/) hello fullständig lista över paket som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="1081c-160">You can search hello [Maven repository](http://search.maven.org/) for hello complete list of packages that are available.</span></span> <span data-ttu-id="1081c-161">Du kan också hämta en lista över tillgängliga paket från andra källor.</span><span class="sxs-lookup"><span data-stu-id="1081c-161">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="1081c-162">Till exempel en fullständig lista över community-bidragit paket finns på [Spark paket](http://spark-packages.org/).</span><span class="sxs-lookup"><span data-stu-id="1081c-162">For example, a complete list of community-contributed packages is available at [Spark Packages](http://spark-packages.org/).</span></span>

<span data-ttu-id="1081c-163">I den här artikeln visas hur toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paket med Jupyter-anteckningsbok hello.</span><span class="sxs-lookup"><span data-stu-id="1081c-163">In this article, you will see how toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package with hello Jupyter notebook.</span></span>

1. <span data-ttu-id="1081c-164">Öppna tolkens inställningar.</span><span class="sxs-lookup"><span data-stu-id="1081c-164">Open interpreter settings.</span></span> <span data-ttu-id="1081c-165">Klicka på hello inloggad användarnamn hello övre högra hörnet och klicka sedan på **tolken**.</span><span class="sxs-lookup"><span data-stu-id="1081c-165">From hello top-right corner, click hello logged in user name, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="1081c-166">![Starta tolken](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive utdata")</span><span class="sxs-lookup"><span data-stu-id="1081c-166">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
2. <span data-ttu-id="1081c-167">Rulla tooLivy tolkens inställningar och klickar sedan på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="1081c-167">Scroll tooLivy interpreter settings and then click **Edit**.</span></span>
   
    <span data-ttu-id="1081c-168">![Ändra inställningarna för tolken](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "ändra tolkens inställningar")</span><span class="sxs-lookup"><span data-stu-id="1081c-168">![Change interpreter settings](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Change interpreter settings")</span></span>
3. <span data-ttu-id="1081c-169">Lägg till en ny nyckel kallas **livy.spark.jars.packages** och ange värdet i hello format `group:id:version`.</span><span class="sxs-lookup"><span data-stu-id="1081c-169">Add a new key, called **livy.spark.jars.packages** and set its value in hello format `group:id:version`.</span></span> <span data-ttu-id="1081c-170">Om du vill toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paketet, måste du ange hello värde hello nyckel för`com.databricks:spark-csv_2.10:1.4.0`.</span><span class="sxs-lookup"><span data-stu-id="1081c-170">So, if you want toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package, you must set hello value of hello key too`com.databricks:spark-csv_2.10:1.4.0`.</span></span>
   
    <span data-ttu-id="1081c-171">![Ändra inställningarna för tolken](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "ändra tolkens inställningar")</span><span class="sxs-lookup"><span data-stu-id="1081c-171">![Change interpreter settings](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Change interpreter settings")</span></span>
   
    <span data-ttu-id="1081c-172">Klicka på **spara** och starta sedan om hello Livius tolken.</span><span class="sxs-lookup"><span data-stu-id="1081c-172">Click **Save** and then restart hello Livy interpreter.</span></span>
4. <span data-ttu-id="1081c-173">**Tips**: Om du vill toounderstand hur tooarrive på hello värdet för hello nyckeln som anges ovan här hur.</span><span class="sxs-lookup"><span data-stu-id="1081c-173">**Tip**: If you want toounderstand how tooarrive at hello value of hello key entered above, here's how.</span></span>
   
    <span data-ttu-id="1081c-174">a.</span><span class="sxs-lookup"><span data-stu-id="1081c-174">a.</span></span> <span data-ttu-id="1081c-175">Leta upp hello paketet i hello Maven-databasen.</span><span class="sxs-lookup"><span data-stu-id="1081c-175">Locate hello package in hello Maven Repository.</span></span> <span data-ttu-id="1081c-176">Den här självstudiekursen kommer vi använde [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span><span class="sxs-lookup"><span data-stu-id="1081c-176">For this tutorial, we used [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span></span>
   
    <span data-ttu-id="1081c-177">b.</span><span class="sxs-lookup"><span data-stu-id="1081c-177">b.</span></span> <span data-ttu-id="1081c-178">Samla in hello värden för från hello databasen **GroupId**, **artefakt-ID**, och **Version**.</span><span class="sxs-lookup"><span data-stu-id="1081c-178">From hello repository, gather hello values for **GroupId**, **ArtifactId**, and **Version**.</span></span>
   
    <span data-ttu-id="1081c-179">![Använda externa paket med Jupyter-anteckningsbok](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "använda externa paket med Jupyter-anteckningsbok")</span><span class="sxs-lookup"><span data-stu-id="1081c-179">![Use external packages with Jupyter notebook](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Use external packages with Jupyter notebook")</span></span>
   
    <span data-ttu-id="1081c-180">c.</span><span class="sxs-lookup"><span data-stu-id="1081c-180">c.</span></span> <span data-ttu-id="1081c-181">Sammanfoga hello tre värden, avgränsade med kolon (**:**).</span><span class="sxs-lookup"><span data-stu-id="1081c-181">Concatenate hello three values, separated by a colon (**:**).</span></span>
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-hello-zeppelin-notebooks-saved"></a><span data-ttu-id="1081c-182">Var finns hello Zeppelin-anteckningsböcker som sparats?</span><span class="sxs-lookup"><span data-stu-id="1081c-182">Where are hello Zeppelin notebooks saved?</span></span>
<span data-ttu-id="1081c-183">Hej Zeppelin-anteckningsböcker sparas toohello klustret headnodes.</span><span class="sxs-lookup"><span data-stu-id="1081c-183">hello Zeppelin notebooks are saved toohello cluster headnodes.</span></span> <span data-ttu-id="1081c-184">Så om du tar bort klustret hello hello anteckningsböcker tas bort även.</span><span class="sxs-lookup"><span data-stu-id="1081c-184">So, if you delete hello cluster, hello notebooks will be deleted as well.</span></span> <span data-ttu-id="1081c-185">Om du vill toopreserve dina anteckningsböcker för senare användning på andra kluster, måste du exportera dem när du har kört hello jobb.</span><span class="sxs-lookup"><span data-stu-id="1081c-185">If you want toopreserve your notebooks for later use on other clusters, you must export them after you have finished running hello jobs.</span></span> <span data-ttu-id="1081c-186">tooexport en bärbar dator, klicka på hello **exportera** ikon som visas i hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="1081c-186">tooexport a notebook, click hello **Export** icon as shown in hello image below.</span></span>

<span data-ttu-id="1081c-187">![Ladda ned anteckningsboken](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Download hello anteckningsboken")</span><span class="sxs-lookup"><span data-stu-id="1081c-187">![Download notebook](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Download hello notebook")</span></span>

<span data-ttu-id="1081c-188">Hello anteckningsboken sparas som en JSON-fil i din nedladdningsplatsen.</span><span class="sxs-lookup"><span data-stu-id="1081c-188">This saves hello notebook as a JSON file in your download location.</span></span>

## <a name="livy-session-management"></a><span data-ttu-id="1081c-189">Livius sessionshantering</span><span class="sxs-lookup"><span data-stu-id="1081c-189">Livy session management</span></span>
<span data-ttu-id="1081c-190">När du kör hello kod punkt i anteckningsboken Zeppelin, skapas en ny session Livius i HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="1081c-190">When you run hello first code paragraph in your Zeppelin notebook, a new Livy session is created in your HDInsight Spark cluster.</span></span> <span data-ttu-id="1081c-191">Den här sessionen delas mellan alla Zeppelin-anteckningsböcker som du skapar.</span><span class="sxs-lookup"><span data-stu-id="1081c-191">This session is shared across all Zeppelin notebooks that you subsequently create.</span></span> <span data-ttu-id="1081c-192">Om du av vissa orsak hello Livius sessionen har avslutats (klustret omstart osv.) kan du inte kan toorun jobb från hello Zeppelin bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="1081c-192">If for some reason hello Livy session is killed (cluster reboot, etc.), you will not be able toorun jobs from hello Zeppelin notebook.</span></span>

<span data-ttu-id="1081c-193">I sådana fall måste du utföra följande steg innan du kan starta jobb som körs från en bärbar dator Zeppelin hello.</span><span class="sxs-lookup"><span data-stu-id="1081c-193">In such a case, you must perform hello following steps before you can start running jobs from a Zeppelin notebook.</span></span> 

1. <span data-ttu-id="1081c-194">Starta om hello Livius tolken från hello Zeppelin anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="1081c-194">Restart hello Livy interpreter from hello Zeppelin notebook.</span></span> <span data-ttu-id="1081c-195">toodo det, öppnar tolkens inställningar genom att klicka på hello inloggad användarnamnet från hello övre högra hörnet och klicka sedan på **tolken**.</span><span class="sxs-lookup"><span data-stu-id="1081c-195">toodo so, open interpreter settings by clicking hello logged in user name from hello top-right corner, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="1081c-196">![Starta tolken](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive utdata")</span><span class="sxs-lookup"><span data-stu-id="1081c-196">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
2. <span data-ttu-id="1081c-197">Rulla tooLivy tolkens inställningar och klickar sedan på **starta om**.</span><span class="sxs-lookup"><span data-stu-id="1081c-197">Scroll tooLivy interpreter settings and then click **Restart**.</span></span>
   
    <span data-ttu-id="1081c-198">![Starta om hello Livius intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "starta om hello Zeppelin intepreter")</span><span class="sxs-lookup"><span data-stu-id="1081c-198">![Restart hello Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Restart hello Zeppelin intepreter")</span></span>
3. <span data-ttu-id="1081c-199">Kör en cell kod från en befintlig Zeppelin-anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="1081c-199">Run a code cell from an existing Zeppelin notebook.</span></span> <span data-ttu-id="1081c-200">Detta skapar en ny session Livius i hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1081c-200">This creates a new Livy session in hello HDInsight cluster.</span></span>

## <span data-ttu-id="1081c-201"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="1081c-201"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="1081c-202">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="1081c-202">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="1081c-203">Scenarier</span><span class="sxs-lookup"><span data-stu-id="1081c-203">Scenarios</span></span>
* [<span data-ttu-id="1081c-204">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="1081c-204">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="1081c-205">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="1081c-205">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="1081c-206">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="1081c-206">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="1081c-207">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="1081c-207">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="1081c-208">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="1081c-208">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="1081c-209">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="1081c-209">Create and run applications</span></span>
* [<span data-ttu-id="1081c-210">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="1081c-210">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="1081c-211">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="1081c-211">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="1081c-212">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="1081c-212">Tools and extensions</span></span>
* [<span data-ttu-id="1081c-213">Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-appar</span><span class="sxs-lookup"><span data-stu-id="1081c-213">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="1081c-214">Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="1081c-214">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="1081c-215">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="1081c-215">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="1081c-216">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="1081c-216">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="1081c-217">Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="1081c-217">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="1081c-218">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="1081c-218">Manage resources</span></span>
* [<span data-ttu-id="1081c-219">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="1081c-219">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="1081c-220">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="1081c-220">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md 







