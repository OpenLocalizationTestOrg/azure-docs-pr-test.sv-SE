---
title: "aaaUse Hadoop Oozie arbetsflöden i Linux-baserat HDInsight | Microsoft Docs"
description: "Använd Hadoop Oozie i Linux-baserade HDInsight. Lär dig hur toodefine ett arbetsflöde för Oozie och skicka ett Oozie-jobb."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d7603471-5076-43d1-8b9a-dbc4e366ce5d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: larryfr
ms.openlocfilehash: cb5682837543312621e3424b7a9341b5d2a00bf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-oozie-with-hadoop-toodefine-and-run-a-workflow-on-linux-based-hdinsight"></a><span data-ttu-id="25759-104">Använda Oozie med Hadoop toodefine och köra ett arbetsflöde på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="25759-104">Use Oozie with Hadoop toodefine and run a workflow on Linux-based HDInsight</span></span>

[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

<span data-ttu-id="25759-105">Lär dig hur toouse Apache Oozie med Hadoop i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="25759-105">Learn how toouse Apache Oozie with Hadoop on HDInsight.</span></span> <span data-ttu-id="25759-106">Apache Oozie är ett arbetsflöde/samordning system som hanterar Hadoop-jobb.</span><span class="sxs-lookup"><span data-stu-id="25759-106">Apache Oozie is a workflow/coordination system that manages Hadoop jobs.</span></span> <span data-ttu-id="25759-107">Oozie är integrerad med hello Hadoop-stacken och stöder hello efter jobb:</span><span class="sxs-lookup"><span data-stu-id="25759-107">Oozie is integrated with hello Hadoop stack, and it supports hello following jobs:</span></span>

* <span data-ttu-id="25759-108">Apache MapReduce</span><span class="sxs-lookup"><span data-stu-id="25759-108">Apache MapReduce</span></span>
* <span data-ttu-id="25759-109">Apache Pig</span><span class="sxs-lookup"><span data-stu-id="25759-109">Apache Pig</span></span>
* <span data-ttu-id="25759-110">Apache Hive</span><span class="sxs-lookup"><span data-stu-id="25759-110">Apache Hive</span></span>
* <span data-ttu-id="25759-111">Apache Sqoop</span><span class="sxs-lookup"><span data-stu-id="25759-111">Apache Sqoop</span></span>

<span data-ttu-id="25759-112">Oozie kan också använda tooschedule jobb som är specifika tooa system, som Java-program eller kommandoskript</span><span class="sxs-lookup"><span data-stu-id="25759-112">Oozie can also be used tooschedule jobs that are specific tooa system, like Java programs or shell scripts</span></span>

> [!NOTE]
> <span data-ttu-id="25759-113">Ett annat alternativ för att definiera arbetsflöden med HDInsight är Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="25759-113">Another option for defining workflows with HDInsight is Azure Data Factory.</span></span> <span data-ttu-id="25759-114">toolearn mer om Azure Data Factory finns [Use Pig och Hive med Data Factory][azure-data-factory-pig-hive].</span><span class="sxs-lookup"><span data-stu-id="25759-114">toolearn more about Azure Data Factory, see [Use Pig and Hive with Data Factory][azure-data-factory-pig-hive].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="25759-115">Oozie har inte aktiverats på domänanslutna HDInsight.</span><span class="sxs-lookup"><span data-stu-id="25759-115">Oozie is not enabled on domain-joined HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25759-116">Krav</span><span class="sxs-lookup"><span data-stu-id="25759-116">Prerequisites</span></span>

* <span data-ttu-id="25759-117">**Ett HDInsight-kluster**: finns [komma igång med HDInsight på Linux](hdinsight-hadoop-linux-tutorial-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="25759-117">**An HDInsight cluster**: See [Get Started with HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md)</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="25759-118">hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="25759-118">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="25759-119">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="25759-119">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="25759-120">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="25759-120">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="example-workflow"></a><span data-ttu-id="25759-121">Arbetsflödesexemplet</span><span class="sxs-lookup"><span data-stu-id="25759-121">Example workflow</span></span>

<span data-ttu-id="25759-122">hello-arbetsflöde som används i det här dokumentet innehåller två åtgärder.</span><span class="sxs-lookup"><span data-stu-id="25759-122">hello workflow used in this document contains two actions.</span></span> <span data-ttu-id="25759-123">Åtgärder är definitioner för uppgifter som att köra Hive, Sqoop, MapReduce eller någon annan process:</span><span class="sxs-lookup"><span data-stu-id="25759-123">Actions are definitions for tasks, such as running Hive, Sqoop, MapReduce, or other process:</span></span>

![Arbetsflödesdiagram][img-workflow-diagram]

1. <span data-ttu-id="25759-125">En Hive-åtgärd körs HiveQL-skript tooextract poster från hello **hivesampletable** ingår i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="25759-125">A Hive action runs a HiveQL script tooextract records from hello **hivesampletable** included with HDInsight.</span></span> <span data-ttu-id="25759-126">Varje rad med data beskriver ett besök av specifika mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="25759-126">Each row of data describes a visit from a specific mobile device.</span></span> <span data-ttu-id="25759-127">hello postformatet visas liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="25759-127">hello record format appears similar toohello following text:</span></span>

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    <span data-ttu-id="25759-128">hello Hive-skript som används i det här dokumentet räknar hello totala besök för varje plattform (till exempel Android eller iPhone) och lagrar hello-antal tooa nya Hive-tabellen.</span><span class="sxs-lookup"><span data-stu-id="25759-128">hello Hive script used in this document counts hello total visits for each platform (such as Android or iPhone) and stores hello counts tooa new Hive table.</span></span>

    <span data-ttu-id="25759-129">Mer information om Hive finns i [Använda Hive med HDInsight][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="25759-129">For more information about Hive, see [Use Hive with HDInsight][hdinsight-use-hive].</span></span>

2. <span data-ttu-id="25759-130">En åtgärd för Sqoop exporterar hello innehållet i hello nya Hive tooa tabellen i en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="25759-130">A Sqoop action exports hello contents of hello new Hive table tooa table in an Azure SQL database.</span></span> <span data-ttu-id="25759-131">Läs mer om Sqoop [Använd Hadoop Sqoop med HDInsight][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="25759-131">For more information about Sqoop, see [Use Hadoop Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

> [!NOTE]
> <span data-ttu-id="25759-132">Versioner som stöds Oozie i HDInsight-kluster, se [vad är nytt i hello Hadoop-klusterversioner som tillhandahålls av HDInsight][hdinsight-versions].</span><span class="sxs-lookup"><span data-stu-id="25759-132">For supported Oozie versions on HDInsight clusters, see [What's new in hello Hadoop cluster versions provided by HDInsight][hdinsight-versions].</span></span>

## <a name="create-hello-working-directory"></a><span data-ttu-id="25759-133">Skapa hello arbetskatalogen</span><span class="sxs-lookup"><span data-stu-id="25759-133">Create hello working directory</span></span>

<span data-ttu-id="25759-134">Oozie förväntar resurser som krävs för ett jobb toobe lagrad i hello samma katalog.</span><span class="sxs-lookup"><span data-stu-id="25759-134">Oozie expects resources required for a job toobe stored in hello same directory.</span></span> <span data-ttu-id="25759-135">Det här exemplet används **wasb: / / / självstudier/useoozie**.</span><span class="sxs-lookup"><span data-stu-id="25759-135">This example uses **wasb:///tutorials/useoozie**.</span></span> <span data-ttu-id="25759-136">Använd hello efter kommandot toocreate den här katalogen och hello data-katalog som innehåller hello nya Hive-tabell skapas av det här arbetsflödet:</span><span class="sxs-lookup"><span data-stu-id="25759-136">Use hello following command toocreate this directory, and hello data directory that holds hello new Hive table created by this workflow:</span></span>

```
hdfs dfs -mkdir -p /tutorials/useoozie/data
```

> [!NOTE]
> <span data-ttu-id="25759-137">Hej `-p` parametern medför att alla kataloger i hello sökvägen toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="25759-137">hello `-p` parameter causes all directories in hello path toobe created.</span></span> <span data-ttu-id="25759-138">Hej **data** katalogen är används toohold data som används av hello **useooziewf.hql** skript.</span><span class="sxs-lookup"><span data-stu-id="25759-138">hello **data** directory is used toohold data used by hello **useooziewf.hql** script.</span></span>

<span data-ttu-id="25759-139">Också köra följande kommando, vilket garanterar att Oozie kan personifiera ditt användarkonto när du kör Hive och Sqoop jobb hello.</span><span class="sxs-lookup"><span data-stu-id="25759-139">Also run hello following command, which ensures that Oozie can impersonate your user account when running Hive and Sqoop jobs.</span></span> <span data-ttu-id="25759-140">Ersätt **användarnamn** med ditt inloggningsnamn:</span><span class="sxs-lookup"><span data-stu-id="25759-140">Replace **USERNAME** with your login name:</span></span>

```
sudo adduser USERNAME users
```

> [!NOTE]
> <span data-ttu-id="25759-141">Du kan ignorera felen hello användaren är redan medlem av hello `users` grupp.</span><span class="sxs-lookup"><span data-stu-id="25759-141">You can ignore errors that hello user is already a member of hello `users` group.</span></span>

## <a name="add-a-database-driver"></a><span data-ttu-id="25759-142">Lägga till en databasdrivrutin</span><span class="sxs-lookup"><span data-stu-id="25759-142">Add a database driver</span></span>

<span data-ttu-id="25759-143">Eftersom det här arbetsflödet använder Sqoop tooexport data tooSQL databasen, måste du ange en kopia av hello JDBC-drivrutinen används tootalk tooSQL databas.</span><span class="sxs-lookup"><span data-stu-id="25759-143">Since this workflow uses Sqoop tooexport data tooSQL Database, you must provide a copy of hello JDBC driver used tootalk tooSQL Database.</span></span> <span data-ttu-id="25759-144">Använd hello följande kommando toocopy den toohello arbetskatalog:</span><span class="sxs-lookup"><span data-stu-id="25759-144">Use hello following command toocopy it toohello working directory:</span></span>

```
hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/
```

<span data-ttu-id="25759-145">Om arbetsflödet används andra resurser, till exempel en jar som innehåller ett MapReduce-program, behöver du tooadd samt dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="25759-145">If your workflow used other resources, such as a jar containing a MapReduce application, you would need tooadd these resources as well.</span></span>

## <a name="define-hello-hive-query"></a><span data-ttu-id="25759-146">Definiera hello Hive-fråga</span><span class="sxs-lookup"><span data-stu-id="25759-146">Define hello Hive query</span></span>

<span data-ttu-id="25759-147">Använd hello följande steg toocreate HiveQL-skript som definierar en fråga som används i ett arbetsflöde för Oozie senare i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="25759-147">Use hello following steps toocreate a HiveQL script that defines a query, which is used in an Oozie workflow later in this document.</span></span>

1. <span data-ttu-id="25759-148">Ansluta toohello kluster med SSH.</span><span class="sxs-lookup"><span data-stu-id="25759-148">Connect toohello cluster using SSH.</span></span> <span data-ttu-id="25759-149">hello följande kommando är ett exempel på hur du använder hello `ssh` kommando.</span><span class="sxs-lookup"><span data-stu-id="25759-149">hello following command is an example of using hello `ssh` command.</span></span> <span data-ttu-id="25759-150">Ersätt __användarnamn__ med hello SSH-användare för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="25759-150">Replace __USERNAME__ with hello SSH user for hello cluster.</span></span> <span data-ttu-id="25759-151">Ersätt __KLUSTERNAMN__ med hello namnet hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="25759-151">Replace __CLUSTERNAME__ with hello name of hello HDInsight cluster.</span></span>

    ```
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="25759-152">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="25759-152">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="25759-153">Använd hello efter kommandot toocreate en fil från hello SSH-anslutningen:</span><span class="sxs-lookup"><span data-stu-id="25759-153">From hello SSH connection, use hello following command toocreate a file:</span></span>

    ```
    nano useooziewf.hql
    ```

3. <span data-ttu-id="25759-154">När hello nanoredigeraren öppnas Använd hello följande fråga som hello innehållet i hello-fil:</span><span class="sxs-lookup"><span data-stu-id="25759-154">Once hello nano editor opens, use hello following query as hello contents of hello file:</span></span>

    ```hiveql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;
    ```

    <span data-ttu-id="25759-155">Det finns två variabler i hello skript:</span><span class="sxs-lookup"><span data-stu-id="25759-155">There are two variables used in hello script:</span></span>

    * <span data-ttu-id="25759-156">**${hiveTableName}**: innehåller hello namnet på hello tabell toobe skapas</span><span class="sxs-lookup"><span data-stu-id="25759-156">**${hiveTableName}**: contains hello name of hello table toobe created</span></span>

    * <span data-ttu-id="25759-157">**${hiveDataFolder}**: innehåller hello plats toostore hello-datafiler för hello tabellen</span><span class="sxs-lookup"><span data-stu-id="25759-157">**${hiveDataFolder}**: contains hello location toostore hello data files for hello table</span></span>

    <span data-ttu-id="25759-158">arbetsflöde för hello-definitionsfil (workflow.xml i den här självstudiekursen) skickar dessa värden toothis HiveQL-skript vid körning.</span><span class="sxs-lookup"><span data-stu-id="25759-158">hello workflow definition file (workflow.xml in this tutorial) passes these values toothis HiveQL script at run time</span></span>

4. <span data-ttu-id="25759-159">tooexit hello redigerare, tryck på Ctrl + X.</span><span class="sxs-lookup"><span data-stu-id="25759-159">tooexit hello editor, press Ctrl-X.</span></span> <span data-ttu-id="25759-160">När du uppmanas, Välj **Y** toosave hello fil och sedan använda **RETUR** toouse hello **useooziewf.hql** filnamn.</span><span class="sxs-lookup"><span data-stu-id="25759-160">When prompted, select **Y** toosave hello file, then use **Enter** toouse hello **useooziewf.hql** file name.</span></span>

5. <span data-ttu-id="25759-161">Använd hello följande kommandon toocopy **useooziewf.hql** för**wasb:///tutorials/useoozie/useooziewf.hql**:</span><span class="sxs-lookup"><span data-stu-id="25759-161">Use hello following commands toocopy **useooziewf.hql** too**wasb:///tutorials/useoozie/useooziewf.hql**:</span></span>

    ```
    hdfs dfs -put useooziewf.hql /tutorials/useoozie/useooziewf.hql
    ```

    <span data-ttu-id="25759-162">Dessa kommandon lagra hello **useooziewf.hql** fil på hello HDFS-kompatibla lagring för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="25759-162">These commands store hello **useooziewf.hql** file on hello HDFS-compatible storage for hello cluster.</span></span>

## <a name="define-hello-workflow"></a><span data-ttu-id="25759-163">Definiera hello-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="25759-163">Define hello workflow</span></span>

<span data-ttu-id="25759-164">Oozie arbetsflöden definitioner skrivs i hPDL (en XML-processen Definition Language).</span><span class="sxs-lookup"><span data-stu-id="25759-164">Oozie workflows definitions are written in hPDL (an XML Process Definition Language).</span></span> <span data-ttu-id="25759-165">Använd följande steg toodefine hello arbetsflödet hello:</span><span class="sxs-lookup"><span data-stu-id="25759-165">Use hello following steps toodefine hello workflow:</span></span>

1. <span data-ttu-id="25759-166">Använd följande instruktion toocreate hello och redigera en ny fil:</span><span class="sxs-lookup"><span data-stu-id="25759-166">Use hello following statement toocreate and edit a new file:</span></span>

    ```
    nano workflow.xml
    ```

2. <span data-ttu-id="25759-167">Ange hello följande XML som hello filinnehållet när hello nanoredigeraren öppnas:</span><span class="sxs-lookup"><span data-stu-id="25759-167">Once hello nano editor opens, enter hello following XML as hello file contents:</span></span>

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start too= "RunHiveScript"/>
        <action name="RunHiveScript">
        <hive xmlns="uri:oozie:hive-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.job.queue.name</name>
                <value>${queueName}</value>
            </property>
            </configuration>
            <script>${hiveScript}</script>
            <param>hiveTableName=${hiveTableName}</param>
            <param>hiveDataFolder=${hiveDataFolder}</param>
        </hive>
        <ok to="RunSqoopExport"/>
        <error to="fail"/>
        </action>
        <action name="RunSqoopExport">
        <sqoop xmlns="uri:oozie:sqoop-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.compress.map.output</name>
                <value>true</value>
            </property>
            </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveDataFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\t"</arg>
            <archive>sqljdbc41.jar</archive>
            </sqoop>
        <ok to="end"/>
        <error to="fail"/>
        </action>
        <kill name="fail">
        <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>
        <end name="end"/>
    </workflow-app>
    ```

    <span data-ttu-id="25759-168">Det finns två åtgärder som definierats i hello arbetsflöde:</span><span class="sxs-lookup"><span data-stu-id="25759-168">There are two actions defined in hello workflow:</span></span>

   * <span data-ttu-id="25759-169">**RunHiveScript**: den här åtgärden är hello start åtgärd och kör hello **useooziewf.hql** registreringsdatafilen</span><span class="sxs-lookup"><span data-stu-id="25759-169">**RunHiveScript**: This action is hello start action, and runs hello **useooziewf.hql** Hive script</span></span>

   * <span data-ttu-id="25759-170">**RunSqoopExport**: den här åtgärden exporterar hello data som har skapats från hello Hive-skript tooSQL databasen med Sqoop.</span><span class="sxs-lookup"><span data-stu-id="25759-170">**RunSqoopExport**: This action exports hello data created from hello Hive script tooSQL Database using Sqoop.</span></span> <span data-ttu-id="25759-171">Den här åtgärden körs bara om hello **RunHiveScript** åtgärden lyckas.</span><span class="sxs-lookup"><span data-stu-id="25759-171">This action only runs if hello **RunHiveScript** action is successful.</span></span>

     <span data-ttu-id="25759-172">hello arbetsflödet har flera transaktioner som `${jobTracker}`.</span><span class="sxs-lookup"><span data-stu-id="25759-172">hello workflow has several entries, such as `${jobTracker}`.</span></span> <span data-ttu-id="25759-173">De här posterna ersätts med värden som du använder i hello jobbdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="25759-173">These entries are replaced by values you use in hello job definition.</span></span> <span data-ttu-id="25759-174">hello jobbdefinitionen skapas senare i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="25759-174">hello job definition is created later in this document.</span></span>

     <span data-ttu-id="25759-175">Även Obs hello `<archive>sqljdbc4.jar</arcive>` post i hello Sqoop avsnitt.</span><span class="sxs-lookup"><span data-stu-id="25759-175">Also note hello `<archive>sqljdbc4.jar</arcive>` entry in hello Sqoop section.</span></span> <span data-ttu-id="25759-176">Den här posten instruerar Oozie toomake det här arkivet för Sqoop när den här åtgärden körs.</span><span class="sxs-lookup"><span data-stu-id="25759-176">This entry instructs Oozie toomake this archive available for Sqoop when this action runs.</span></span>

3. <span data-ttu-id="25759-177">Använd Ctrl + X sedan **Y** och **RETUR** toosave hello-filen.</span><span class="sxs-lookup"><span data-stu-id="25759-177">Use Ctrl-X, then **Y** and **Enter** toosave hello file.</span></span>

4. <span data-ttu-id="25759-178">Använd hello följande kommando toocopy hello **workflow.xml** filen för**/tutorials/useoozie/workflow.xml**:</span><span class="sxs-lookup"><span data-stu-id="25759-178">Use hello following command toocopy hello **workflow.xml** file too**/tutorials/useoozie/workflow.xml**:</span></span>

    ```
    hdfs dfs -put workflow.xml /tutorials/useoozie/workflow.xml
    ```

## <a name="create-hello-database"></a><span data-ttu-id="25759-179">Skapa hello-databas</span><span class="sxs-lookup"><span data-stu-id="25759-179">Create hello database</span></span>

<span data-ttu-id="25759-180">toocreate en Azure SQL Database så hello i hello [skapa en SQL-databas](../sql-database/sql-database-get-started.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="25759-180">toocreate an Azure SQL Database, follow hello steps in hello [Create a SQL Database](../sql-database/sql-database-get-started.md) document.</span></span> <span data-ttu-id="25759-181">När du skapar hello databasen använder `oozietest` som hello databasnamn.</span><span class="sxs-lookup"><span data-stu-id="25759-181">When creating hello database, use `oozietest` as hello database name.</span></span> <span data-ttu-id="25759-182">Även anteckna hello namnet på hello databasserver.</span><span class="sxs-lookup"><span data-stu-id="25759-182">Also make a note of hello name of hello database server.</span></span>

### <a name="create-hello-table"></a><span data-ttu-id="25759-183">Skapa hello tabell</span><span class="sxs-lookup"><span data-stu-id="25759-183">Create hello table</span></span>

> [!NOTE]
> <span data-ttu-id="25759-184">Det finns många sätt tooconnect tooSQL databasen toocreate en tabell.</span><span class="sxs-lookup"><span data-stu-id="25759-184">There are many ways tooconnect tooSQL Database toocreate a table.</span></span> <span data-ttu-id="25759-185">Hej följande steg används [FreeTDS](http://www.freetds.org/) från hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="25759-185">hello following steps use [FreeTDS](http://www.freetds.org/) from hello HDInsight cluster.</span></span>


1. <span data-ttu-id="25759-186">Använd hello efter kommandot tooinstall FreeTDS på hello HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="25759-186">Use hello following command tooinstall FreeTDS on hello HDInsight cluster:</span></span>

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. <span data-ttu-id="25759-187">När du har installerat FreeTDS kommando Använd hello följande tooconnect toohello SQL Database-server som du skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="25759-187">Once FreeTDS has been installed, use hello following command tooconnect toohello SQL Database server you created previously:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest
    ```

    <span data-ttu-id="25759-188">Du får utdata liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="25759-188">You receive output similar toohello following text:</span></span>

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set toooozietest
        1>

3. <span data-ttu-id="25759-189">Vid hello `1>` uppmanar, ange hello följande rader:</span><span class="sxs-lookup"><span data-stu-id="25759-189">At hello `1>` prompt, enter hello following lines:</span></span>

    ```
    CREATE TABLE [dbo].[mobiledata](
    [deviceplatform] [nvarchar](50),
    [count] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
    GO
    ```

    <span data-ttu-id="25759-190">När hello `GO` uttryck har angetts, hello tidigare rapporter utvärderas.</span><span class="sxs-lookup"><span data-stu-id="25759-190">When hello `GO` statement is entered, hello previous statements are evaluated.</span></span> <span data-ttu-id="25759-191">De här uttrycken skapa en tabell med namnet **mobiledata** som används av hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="25759-191">These statements create a table named **mobiledata** that is used by hello workflow.</span></span>

    <span data-ttu-id="25759-192">Använd hello följande tooverify som hello tabellen har skapats:</span><span class="sxs-lookup"><span data-stu-id="25759-192">Use hello following tooverify that hello table has been created:</span></span>

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="25759-193">Du kan se utdata liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="25759-193">You see output similar toohello following text:</span></span>

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    oozietest       dbo     mobiledata      BASE TABLE
    ```

4. <span data-ttu-id="25759-194">Ange `exit` på hello `1>` fråga tooexit hello tsql-verktyget.</span><span class="sxs-lookup"><span data-stu-id="25759-194">Enter `exit` at hello `1>` prompt tooexit hello tsql utility.</span></span>

## <a name="create-hello-job-definition"></a><span data-ttu-id="25759-195">Skapa hello jobbdefinitionen</span><span class="sxs-lookup"><span data-stu-id="25759-195">Create hello job definition</span></span>

<span data-ttu-id="25759-196">hello jobbdefinitionen beskriver där toofind hello workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="25759-196">hello job definition describes where toofind hello workflow.xml.</span></span> <span data-ttu-id="25759-197">Här beskrivs också var toofind andra filer som används av hello arbetsflöde (till exempel useooziewf.hql.) Den definierar även hello värden för egenskaper som används i hello arbetsflöde och tillhörande filer.</span><span class="sxs-lookup"><span data-stu-id="25759-197">It also describes where toofind other files used by hello workflow (such as useooziewf.hql.) It also defines hello values for properties used within hello workflow and associated files.</span></span>

1. <span data-ttu-id="25759-198">Använd följande kommando tooget hello fullständiga adress hello standardlagring hello.</span><span class="sxs-lookup"><span data-stu-id="25759-198">Use hello following command tooget hello full address of hello default storage.</span></span> <span data-ttu-id="25759-199">Den här adressen används i konfigurationsfilen för hello strax:</span><span class="sxs-lookup"><span data-stu-id="25759-199">This address is used in hello configuration file in a moment:</span></span>

    ```
    sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml
    ```

    <span data-ttu-id="25759-200">Det här kommandot returnerar informationen liknande toohello följande XML:</span><span class="sxs-lookup"><span data-stu-id="25759-200">This command returns information similar toohello following XML:</span></span>

    ```xml
    <name>fs.defaultFS</name>
    <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
    ```

    > [!NOTE]
    > <span data-ttu-id="25759-201">Om hello HDInsight-kluster använder Azure Storage som hello standardlagring, hello `<value>` element innehållet börjar med `wasb://`.</span><span class="sxs-lookup"><span data-stu-id="25759-201">If hello HDInsight cluster uses Azure Storage as hello default storage, hello `<value>` element contents begin with `wasb://`.</span></span> <span data-ttu-id="25759-202">Om Azure Data Lake Store används i stället, den börjar med `adl://`.</span><span class="sxs-lookup"><span data-stu-id="25759-202">If Azure Data Lake Store is used instead, it begins with `adl://`.</span></span>

    <span data-ttu-id="25759-203">Spara hello innehållet i hello `<value>` elementet eftersom den används i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="25759-203">Save hello contents of hello `<value>` element, as it is used in hello next steps.</span></span>

2. <span data-ttu-id="25759-204">Använd följande kommando tooget FQDN för hello klustret headnode hello.</span><span class="sxs-lookup"><span data-stu-id="25759-204">Use hello following command tooget FQDN of hello cluster headnode.</span></span> <span data-ttu-id="25759-205">Den här informationen används för hello JobTracker adress för hello klustret:</span><span class="sxs-lookup"><span data-stu-id="25759-205">This information is used for hello JobTracker address for hello cluster:</span></span>

    ```
    hostname -f
    ```

    <span data-ttu-id="25759-206">Detta returnerar information liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="25759-206">This returns information similar toohello following text:</span></span>

    ```hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net```

    <span data-ttu-id="25759-207">hello port som används för hello JobTracker är 8050, så hello fullständiga adress toouse för hello JobTracker `hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050`.</span><span class="sxs-lookup"><span data-stu-id="25759-207">hello port used for hello JobTracker is 8050, so hello full address toouse for hello JobTracker is `hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050`.</span></span>

3. <span data-ttu-id="25759-208">Använd hello följande toocreate hello Oozie jobbet definition konfiguration:</span><span class="sxs-lookup"><span data-stu-id="25759-208">Use hello following toocreate hello Oozie job definition configuration:</span></span>

    ```
    nano job.xml
    ```

4. <span data-ttu-id="25759-209">När hello nanoredigeraren öppnas Använd hello följande XML som hello innehållet i hello-fil:</span><span class="sxs-lookup"><span data-stu-id="25759-209">Once hello nano editor opens, use hello following XML as hello contents of hello file:</span></span>

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
        <name>nameNode</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
        </property>

        <property>
        <name>jobTracker</name>
        <value>JOBTRACKERADDRESS</value>
        </property>

        <property>
        <name>queueName</name>
        <value>default</value>
        </property>

        <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
        </property>

        <property>
        <name>hiveScript</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
        </property>

        <property>
        <name>hiveTableName</name>
        <value>mobilecount</value>
        </property>

        <property>
        <name>hiveDataFolder</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
        </property>

        <property>
        <name>sqlDatabaseConnectionString</name>
        <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
        </property>

        <property>
        <name>sqlDatabaseTableName</name>
        <value>mobiledata</value>
        </property>

        <property>
        <name>user.name</name>
        <value>YourName</value>
        </property>

        <property>
        <name>oozie.wf.application.path</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
    </configuration>
    ```

   * <span data-ttu-id="25759-210">Ersätt alla förekomster av  **wasb://mycontainer@mystorageaccount.blob.core.windows.net**  med hello-värde som du fick tidigare för standardlagring.</span><span class="sxs-lookup"><span data-stu-id="25759-210">Replace all instances of **wasb://mycontainer@mystorageaccount.blob.core.windows.net** with hello value you received earlier for default storage.</span></span>

     > [!WARNING]
     > <span data-ttu-id="25759-211">Om hello sökvägen är en `wasb` sökväg, måste du använda hello fullständig sökväg.</span><span class="sxs-lookup"><span data-stu-id="25759-211">If hello path is a `wasb` path, you must use hello full path.</span></span> <span data-ttu-id="25759-212">Inte förkorta toojust `wasb:///`.</span><span class="sxs-lookup"><span data-stu-id="25759-212">Do not shorten it toojust `wasb:///`.</span></span>

   * <span data-ttu-id="25759-213">Ersätt **JOBTRACKERADDRESS** med hello JobTracker/ResourceManager-adress som du fått tidigare.</span><span class="sxs-lookup"><span data-stu-id="25759-213">Replace **JOBTRACKERADDRESS** with hello JobTracker/ResourceManager address you received earlier.</span></span>
   * <span data-ttu-id="25759-214">Ersätt **dittnamn** med ditt inloggningsnamn för hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="25759-214">Replace **YourName** with your login name for hello HDInsight cluster.</span></span>
   * <span data-ttu-id="25759-215">Ersätt **serverName**, **adminLogin**, och **adminPassword** med hello information för din Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="25759-215">Replace **serverName**, **adminLogin**, and **adminPassword** with hello information for your Azure SQL Database.</span></span>

     <span data-ttu-id="25759-216">I de flesta hello informationen i filen är används toopopulate hello värden som används i hello workflow.xml eller ooziewf.hql filer (till exempel ${nameNode}.)</span><span class="sxs-lookup"><span data-stu-id="25759-216">Most of hello information in this file is used toopopulate hello values used in hello workflow.xml or ooziewf.hql files (such as ${nameNode}.)</span></span>

     > [!NOTE]
     > <span data-ttu-id="25759-217">Hej **oozie.wf.application.path** post definierar var toofind hello workflow.xml filen som innehåller hello arbetsflödet kördes av jobbet.</span><span class="sxs-lookup"><span data-stu-id="25759-217">hello **oozie.wf.application.path** entry defines where toofind hello workflow.xml file, which contains hello workflow ran by this job.</span></span>

5. <span data-ttu-id="25759-218">Använd Ctrl + X sedan **Y** och **RETUR** toosave hello-filen.</span><span class="sxs-lookup"><span data-stu-id="25759-218">Use Ctrl-X, then **Y** and **Enter** toosave hello file.</span></span>

## <a name="submit-and-manage-hello-job"></a><span data-ttu-id="25759-219">Skicka in och hantera hello jobb</span><span class="sxs-lookup"><span data-stu-id="25759-219">Submit and manage hello job</span></span>

<span data-ttu-id="25759-220">hello följande använda hello Oozie kommandot toosubmit och hantera Oozie arbetsflöden på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="25759-220">hello following steps use hello Oozie command toosubmit and manage Oozie workflows on hello cluster.</span></span> <span data-ttu-id="25759-221">Hej Oozie kommando är ett gränssnitt för egna över hello [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span><span class="sxs-lookup"><span data-stu-id="25759-221">hello Oozie command is a friendly interface over hello [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="25759-222">När kommandot hello Oozie måste du använda hello FQDN för hello HDInsight headnode.</span><span class="sxs-lookup"><span data-stu-id="25759-222">When using hello Oozie command, you must use hello FQDN for hello HDInsight headnode.</span></span> <span data-ttu-id="25759-223">Detta FQDN är enbart tillgänglig från hello kluster, eller om hello klustret på ett virtuellt Azure-nätverk från andra datorer på hello samma nätverk.</span><span class="sxs-lookup"><span data-stu-id="25759-223">This FQDN is only accessible from hello cluster, or if hello cluster is on an Azure Virtual Network, from other machines on hello same network.</span></span>


1. <span data-ttu-id="25759-224">Använd följande tooobtain hello URL toohello Oozie service hello:</span><span class="sxs-lookup"><span data-stu-id="25759-224">Use hello following tooobtain hello URL toohello Oozie service:</span></span>

    ```
    sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml
    ```

    <span data-ttu-id="25759-225">Detta returnerar information liknande toohello följande XML:</span><span class="sxs-lookup"><span data-stu-id="25759-225">This returns information similar toohello following XML:</span></span>

    ```xml
    <name>oozie.base.url</name>
    <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>
    ```

    <span data-ttu-id="25759-226">Hej `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` del är hello URL toouse med hello Oozie-kommando.</span><span class="sxs-lookup"><span data-stu-id="25759-226">hello `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` portion is hello URL toouse with hello Oozie command.</span></span>

2. <span data-ttu-id="25759-227">Använd hello följande toocreate en miljövariabel för hello-URL så att du inte behöver tootype för varje kommando:</span><span class="sxs-lookup"><span data-stu-id="25759-227">Use hello following toocreate an environment variable for hello URL, so you don't have tootype it for every command:</span></span>

    ```
    export OOZIE_URL=http://HOSTNAMEt:11000/oozie
    ```

    <span data-ttu-id="25759-228">Ersätt hello URL med hello en tidigare.</span><span class="sxs-lookup"><span data-stu-id="25759-228">Replace hello URL with hello one you received earlier.</span></span>
3. <span data-ttu-id="25759-229">Använd hello följande toosubmit hello jobb:</span><span class="sxs-lookup"><span data-stu-id="25759-229">Use hello following toosubmit hello job:</span></span>

    ```
    oozie job -config job.xml -submit
    ```

    <span data-ttu-id="25759-230">Det här kommandot laddar hello jobbinformation från **job.xml** och skickar den tooOozie, men har inte köra den.</span><span class="sxs-lookup"><span data-stu-id="25759-230">This command loads hello job information from **job.xml** and submits it tooOozie, but does not run it.</span></span>

    <span data-ttu-id="25759-231">Det ska returnera hello-ID för hello jobbet när hello-kommandot har slutförts.</span><span class="sxs-lookup"><span data-stu-id="25759-231">Once hello command completes, it should return hello ID of hello job.</span></span> <span data-ttu-id="25759-232">Till exempel `0000005-150622124850154-oozie-oozi-W`.</span><span class="sxs-lookup"><span data-stu-id="25759-232">For example, `0000005-150622124850154-oozie-oozi-W`.</span></span> <span data-ttu-id="25759-233">Detta ID är används toomanage hello jobb.</span><span class="sxs-lookup"><span data-stu-id="25759-233">This ID is used toomanage hello job.</span></span>

4. <span data-ttu-id="25759-234">Visa hello status för hello jobb med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="25759-234">View hello status of hello job using hello following command:</span></span>

    ```
    oozie job -info <JOBID>
    ```

    > [!NOTE]
    > <span data-ttu-id="25759-235">Ersätt `<JOBID>` med hello-ID som returneras i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="25759-235">Replace `<JOBID>` with hello ID returned in hello previous step.</span></span>

    <span data-ttu-id="25759-236">Detta returnerar information liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="25759-236">This returns information similar toohello following text:</span></span>

    ```
    Job ID : 0000005-150622124850154-oozie-oozi-W
    ------------------------------------------------------------------------------------------------------------------------------------
    Workflow Name : useooziewf
    App Path      : wasb:///tutorials/useoozie
    Status        : PREP
    Run           : 0
    User          : USERNAME
    Group         : -
    Created       : 2015-06-22 15:06 GMT
    Started       : -
    Last Modified : 2015-06-22 15:06 GMT
    Ended         : -
    CoordAction ID: -
    ------------------------------------------------------------------------------------------------------------------------------------
    ```

    <span data-ttu-id="25759-237">Det här jobbet har statusen `PREP`.</span><span class="sxs-lookup"><span data-stu-id="25759-237">This job has a status of `PREP`.</span></span> <span data-ttu-id="25759-238">Denna status anger hello jobbet har skapats, men inte har startats.</span><span class="sxs-lookup"><span data-stu-id="25759-238">This status indicates that hello job was created, but not started.</span></span>

5. <span data-ttu-id="25759-239">Använd hello följande kommando toostart hello jobb:</span><span class="sxs-lookup"><span data-stu-id="25759-239">Use hello following command toostart hello job:</span></span>

    ```
    oozie job -start JOBID
    ```

    > [!NOTE]
    > <span data-ttu-id="25759-240">Ersätt `<JOBID>` med hello-ID som returneras tidigare.</span><span class="sxs-lookup"><span data-stu-id="25759-240">Replace `<JOBID>` with hello ID returned previously.</span></span>

    <span data-ttu-id="25759-241">Om du kontaktar hello status efter kommandot, den är i ett körningstillstånd och information returneras för hello åtgärder inom hello jobb.</span><span class="sxs-lookup"><span data-stu-id="25759-241">If you check hello status after this command, it is in a running state, and information is returned for hello actions within hello job.</span></span>

6. <span data-ttu-id="25759-242">När hello aktiviteten är klar kan du kontrollera att hello data genererades och exporterade toohello SQL Database-tabellen med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="25759-242">Once hello task completes successfully, you can verify that hello data was generated and exported toohello SQL Database table by using hello following commands:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest
    ```

    <span data-ttu-id="25759-243">Vid hello `1>` uppmanar, ange hello följande fråga:</span><span class="sxs-lookup"><span data-stu-id="25759-243">At hello `1>` prompt, enter hello following query:</span></span>

    ```
    SELECT * FROM mobiledata
    GO
    ```

    <span data-ttu-id="25759-244">hello information som returnerades är liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="25759-244">hello information returned is similar toohello following text:</span></span>

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

<span data-ttu-id="25759-245">Mer information om hello Oozie kommandot finns [Oozie kommandoradsverktyget](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).</span><span class="sxs-lookup"><span data-stu-id="25759-245">For more information on hello Oozie command, see [Oozie command-line tool](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).</span></span>

## <a name="oozie-rest-api"></a><span data-ttu-id="25759-246">Oozie REST API</span><span class="sxs-lookup"><span data-stu-id="25759-246">Oozie REST API</span></span>

<span data-ttu-id="25759-247">Hej Oozie REST-API kan du toobuild egna verktyg som fungerar med Oozie.</span><span class="sxs-lookup"><span data-stu-id="25759-247">hello Oozie REST API allows you toobuild your own tools that work with Oozie.</span></span> <span data-ttu-id="25759-248">hello följande är HDInsight specifik information om hur du använder hello Oozie REST API:</span><span class="sxs-lookup"><span data-stu-id="25759-248">hello following are HDInsight specific information about using hello Oozie REST API:</span></span>

* <span data-ttu-id="25759-249">**URI**: hello REST-API som kan nås från utanför hello klustret på`https://CLUSTERNAME.azurehdinsight.net/oozie`</span><span class="sxs-lookup"><span data-stu-id="25759-249">**URI**: hello REST API can be accessed from outside hello cluster at `https://CLUSTERNAME.azurehdinsight.net/oozie`</span></span>

* <span data-ttu-id="25759-250">**Autentisering**: autentisera toohello API hello klustret HTTP-konto (admin) och lösenord.</span><span class="sxs-lookup"><span data-stu-id="25759-250">**Authentication**: Authenticate toohello API using hello cluster HTTP account (admin) and password.</span></span> <span data-ttu-id="25759-251">Exempel:</span><span class="sxs-lookup"><span data-stu-id="25759-251">For example:</span></span>

    ```
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions
    ```

<span data-ttu-id="25759-252">Mer information om hur du använder hello Oozie REST-API finns [Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span><span class="sxs-lookup"><span data-stu-id="25759-252">For more information on using hello Oozie REST API, see [Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span></span>

## <a name="oozie-web-ui"></a><span data-ttu-id="25759-253">Oozie webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="25759-253">Oozie Web UI</span></span>

<span data-ttu-id="25759-254">Hej Oozie Webbgränssnittet kan du öppna webbaserade hello status för Oozie-jobb på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="25759-254">hello Oozie Web UI provides a web-based view into hello status of Oozie jobs on hello cluster.</span></span> <span data-ttu-id="25759-255">hello webbgränssnittet kan tooview hello följande information:</span><span class="sxs-lookup"><span data-stu-id="25759-255">hello web UI allows you tooview hello following information:</span></span>

* <span data-ttu-id="25759-256">Jobbstatus</span><span class="sxs-lookup"><span data-stu-id="25759-256">Job status</span></span>
* <span data-ttu-id="25759-257">Jobbdefinitionen</span><span class="sxs-lookup"><span data-stu-id="25759-257">Job definition</span></span>
* <span data-ttu-id="25759-258">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="25759-258">Configuration</span></span>
* <span data-ttu-id="25759-259">Ett diagram över hello åtgärder i hello jobb</span><span class="sxs-lookup"><span data-stu-id="25759-259">A graph of hello actions in hello job</span></span>
* <span data-ttu-id="25759-260">Loggar för jobbet hello</span><span class="sxs-lookup"><span data-stu-id="25759-260">Logs for hello job</span></span>

<span data-ttu-id="25759-261">Du kan också visa information om åtgärder i ett jobb.</span><span class="sxs-lookup"><span data-stu-id="25759-261">You can also view details for actions within a job.</span></span>

<span data-ttu-id="25759-262">tooaccess hello Oozie Webbgränssnittet använder hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="25759-262">tooaccess hello Oozie Web UI, use hello following steps:</span></span>

1. <span data-ttu-id="25759-263">Skapa en SSH-tunnel toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="25759-263">Create an SSH tunnel toohello HDInsight cluster.</span></span> <span data-ttu-id="25759-264">Mer information finns i hello [använda SSH-tunnlar med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="25759-264">For information, see hello [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

2. <span data-ttu-id="25759-265">När en tunnel har skapats, öppnar du hello Ambari-webbgränssnittet i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="25759-265">Once a tunnel has been created, open hello Ambari web UI in your web browser.</span></span> <span data-ttu-id="25759-266">hello URI för hello Ambari plats är **https://CLUSTERNAME.azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="25759-266">hello URI for hello Ambari site is **https://CLUSTERNAME.azurehdinsight.net**.</span></span> <span data-ttu-id="25759-267">Ersätt **KLUSTERNAMN** med hello namnet på ditt Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="25759-267">Replace **CLUSTERNAME** with hello name of your Linux-based HDInsight cluster.</span></span>

3. <span data-ttu-id="25759-268">Hello vänster sida av hello-sidan, Välj **Oozie**, sedan **snabblänkar**, och slutligen **Oozie Webbgränssnittet**.</span><span class="sxs-lookup"><span data-stu-id="25759-268">From hello left side of hello page, select **Oozie**, then **Quick Links**, and finally **Oozie Web UI**.</span></span>

    ![Bild av hello menyer](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. <span data-ttu-id="25759-270">Hej Oozie Webbgränssnittet standardvärden toodisplaying kör arbetsflödesjobb.</span><span class="sxs-lookup"><span data-stu-id="25759-270">hello Oozie Web UI defaults toodisplaying running Workflow Jobs.</span></span> <span data-ttu-id="25759-271">toosee alla arbetsflödesuppgifter, Välj **alla jobb**.</span><span class="sxs-lookup"><span data-stu-id="25759-271">toosee all workflow jobs, select **All Jobs**.</span></span>

    ![Alla jobb som visas](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. <span data-ttu-id="25759-273">Välj ett jobb tooview mer information om hello jobb.</span><span class="sxs-lookup"><span data-stu-id="25759-273">Select a job tooview more information about hello job.</span></span>

    ![Jobbinformationen](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. <span data-ttu-id="25759-275">Du kan se grundläggande jobbinformation och hello enskilda åtgärder i hello jobb från hello Jobbinformationen fliken.</span><span class="sxs-lookup"><span data-stu-id="25759-275">From hello Job Info tab, you can see basic job information and hello individual actions within hello job.</span></span> <span data-ttu-id="25759-276">Med hjälp av hello flikarna överst hello kan du visa hello jobbdefinitionen, jobb Configuration hello åtkomst till Jobblogg eller visa en dirigeras acykliska diagram (DAG) för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="25759-276">Using hello tabs at hello top you can view hello Job Definition, Job Configuration, access hello Job Log or view a Directed Acyclic Graph (DAG) of hello job.</span></span>

   * <span data-ttu-id="25759-277">**Jobblogg**: Välj hello **GetLogs** tooget alla loggar för jobbet hello, eller använda hello **ange sökfilter** fältet toofilter loggar</span><span class="sxs-lookup"><span data-stu-id="25759-277">**Job Log**: Select hello **GetLogs** button tooget all logs for hello job, or use hello **Enter Search Filter** field toofilter logs</span></span>

       ![Jobblogg](./media/hdinsight-use-oozie-linux-mac/joblog.png)

   * <span data-ttu-id="25759-279">**JobDAG**: hello DAG är en grafisk översikt över hello datasökvägar vidtas via hello-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="25759-279">**JobDAG**: hello DAG is a graphical overview of hello data paths taken through hello workflow</span></span>

       ![Jobbet DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. <span data-ttu-id="25759-281">Att välja något av hello hello **Jobbinformationen** fliken visar information om hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="25759-281">Selecting one of hello actions from hello **Job Info** tab brings up information for hello action.</span></span> <span data-ttu-id="25759-282">Välj exempelvis hello **RunHiveScript** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="25759-282">For example, select hello **RunHiveScript** action.</span></span>

    ![Åtgärden info](./media/hdinsight-use-oozie-linux-mac/action.png)

8. <span data-ttu-id="25759-284">Du kan visa detaljer för hello-åtgärden, till exempel en länk toohello **-konsolens URL**.</span><span class="sxs-lookup"><span data-stu-id="25759-284">You can see details for hello action, such as a link toohello **Console URL**.</span></span> <span data-ttu-id="25759-285">Den här länken kan vara används tooview JobTracker information för hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="25759-285">This link can be used tooview JobTracker information for hello job.</span></span>

## <a name="scheduling-jobs"></a><span data-ttu-id="25759-286">Schemalägger jobb</span><span class="sxs-lookup"><span data-stu-id="25759-286">Scheduling jobs</span></span>

<span data-ttu-id="25759-287">hello coordinator kan toospecify en start och end förekomsten frekvens för jobb.</span><span class="sxs-lookup"><span data-stu-id="25759-287">hello coordinator allows you toospecify a start, end, and occurrence frequency for jobs.</span></span> <span data-ttu-id="25759-288">toodefine ett schema för hello arbetsflöde, Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="25759-288">toodefine a schedule for hello workflow, use hello following steps:</span></span>

1. <span data-ttu-id="25759-289">Använd hello följande toocreate en fil med namnet **coordinator.xml**:</span><span class="sxs-lookup"><span data-stu-id="25759-289">Use hello following toocreate a file named **coordinator.xml**:</span></span>

    ```
    nano coordinator.xml
    ```

    <span data-ttu-id="25759-290">Använd hello följande XML som hello innehållet i hello-fil:</span><span class="sxs-lookup"><span data-stu-id="25759-290">Use hello following XML as hello contents of hello file:</span></span>

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
        <workflow>
            <app-path>${workflowPath}</app-path>
        </workflow>
        </action>
    </coordinator-app>
    ```

    > [!NOTE]
    > <span data-ttu-id="25759-291">Hej `${...}` variabler ersätts med värden i hello jobbdefinitionen vid körning.</span><span class="sxs-lookup"><span data-stu-id="25759-291">hello `${...}` variables are replaced by values in hello job definition at run-time.</span></span> <span data-ttu-id="25759-292">hello-variabler är:</span><span class="sxs-lookup"><span data-stu-id="25759-292">hello variables are:</span></span>
    >
    > * <span data-ttu-id="25759-293">`${coordFrequency}`: Tiden mellan instanser av hello jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="25759-293">`${coordFrequency}`: Time between running instances of hello job.</span></span>
    > <span data-ttu-id="25759-294">** `${coordStart}`: hello jobbets starttid.</span><span class="sxs-lookup"><span data-stu-id="25759-294">** `${coordStart}`: hello job start time.</span></span>
    > * <span data-ttu-id="25759-295">`${coordEnd}`: hello jobbets sluttid.</span><span class="sxs-lookup"><span data-stu-id="25759-295">`${coordEnd}`: hello job end time.</span></span>
    > * <span data-ttu-id="25759-296">`${coordTimezone}`: Coordinator jobb är i en fast tidszon med ingen sommartid (vanligtvis representeras med hjälp av UTC).</span><span class="sxs-lookup"><span data-stu-id="25759-296">`${coordTimezone}`: Coordinator jobs are in a fixed time zone with no daylight savings time (typically represented by using UTC).</span></span> <span data-ttu-id="25759-297">Den här tidszonen kallas hello ”Oozie bearbetning tidszonen”.</span><span class="sxs-lookup"><span data-stu-id="25759-297">This time zone is referred as hello "Oozie processing timezone."</span></span>
    > * <span data-ttu-id="25759-298">`${wfPath}`: hello sökvägen toohello workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="25759-298">`${wfPath}`: hello path toohello workflow.xml.</span></span>

2. <span data-ttu-id="25759-299">toosave hello fil ska du använda Ctrl + X **Y**, och **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="25759-299">toosave hello file, use Ctrl-X, **Y**, and **Enter**.</span></span>

3. <span data-ttu-id="25759-300">Använd hello efter kommandot toocopy hello filen toohello arbetskatalog för det här jobbet:</span><span class="sxs-lookup"><span data-stu-id="25759-300">Use hello following command toocopy hello file toohello working directory for this job:</span></span>

    ```
    hadoop fs -put coordinator.xml /tutorials/useoozie/coordinator.xml
    ```

4. <span data-ttu-id="25759-301">Använd hello följande toomodify hello **job.xml** fil:</span><span class="sxs-lookup"><span data-stu-id="25759-301">Use hello following toomodify hello **job.xml** file:</span></span>

    ```
    nano job.xml
    ```

    <span data-ttu-id="25759-302">Att Hej följande ändringar:</span><span class="sxs-lookup"><span data-stu-id="25759-302">Make hello following changes:</span></span>

   * <span data-ttu-id="25759-303">tooinstruct oozie toorun hello coordinator fil i stället för hello arbetsflöde, ändra `<name>oozie.wf.application.path</name>` för`<name>oozie.coord.application.path</name>`.</span><span class="sxs-lookup"><span data-stu-id="25759-303">tooinstruct oozie toorun hello coordinator file instead of hello workflow, change `<name>oozie.wf.application.path</name>` too`<name>oozie.coord.application.path</name>`.</span></span>

   * <span data-ttu-id="25759-304">tooset hello `workflowPath` variabeln som används av hello coordinator lägga till hello följande XML:</span><span class="sxs-lookup"><span data-stu-id="25759-304">tooset hello `workflowPath` variable used by hello coordinator, add hello following XML:</span></span>

        ```xml
        <property>
            <name>workflowPath</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
        ```

       <span data-ttu-id="25759-305">Ersätt hello `wasb://mycontainer@mystorageaccount.blob.core.windows` text med hello-värde som används i andra transaktioner i hello job.xml-filen.</span><span class="sxs-lookup"><span data-stu-id="25759-305">Replace hello `wasb://mycontainer@mystorageaccount.blob.core.windows` text with hello value used in other entries in hello job.xml file.</span></span>

   * <span data-ttu-id="25759-306">Lägg till hello följande XML toodefine hello start och end frekvens för hello coordinator:</span><span class="sxs-lookup"><span data-stu-id="25759-306">toodefine hello start, end, and frequency for hello coordinator, add hello following XML:</span></span>

        ```xml
        <property>
            <name>coordStart</name>
            <value>2017-05-10T12:00Z</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>2017-05-12T12:00Z</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>1440</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>UTC</value>
        </property>
        ```

       <span data-ttu-id="25759-307">Dessa värden anges hello start tid too12: 00 PM på 10 maj 2017 hello slutet tid tooMay 12, 2017.</span><span class="sxs-lookup"><span data-stu-id="25759-307">These values set hello start time too12:00PM on May 10, 2017, hello end time tooMay 12, 2017.</span></span> <span data-ttu-id="25759-308">hello-intervall för det här jobbet körs varje dag.</span><span class="sxs-lookup"><span data-stu-id="25759-308">hello interval for running this job daily.</span></span> <span data-ttu-id="25759-309">hello frekvensen är i minuter, så 24 timmar x 60 minuter = 1440 minuter.</span><span class="sxs-lookup"><span data-stu-id="25759-309">hello frequency is in minutes, so 24 hours x 60 minutes = 1440 minutes.</span></span> <span data-ttu-id="25759-310">Slutligen anges hello tidszonen tooUTC.</span><span class="sxs-lookup"><span data-stu-id="25759-310">Finally, hello timezone is set tooUTC.</span></span>

5. <span data-ttu-id="25759-311">Använd Ctrl + X sedan **Y** och **RETUR** toosave hello-filen.</span><span class="sxs-lookup"><span data-stu-id="25759-311">Use Ctrl-X, then **Y** and **Enter** toosave hello file.</span></span>

6. <span data-ttu-id="25759-312">toorun hello jobbet, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="25759-312">toorun hello job, use hello following command:</span></span>

    ```
    oozie job -config job.xml -run
    ```

    <span data-ttu-id="25759-313">Det här kommandot skickar och startar hello jobb.</span><span class="sxs-lookup"><span data-stu-id="25759-313">This command submits and starts hello job.</span></span>

7. <span data-ttu-id="25759-314">Om du besöker hello Oozie Webbgränssnittet och välj hello **Coordinator jobb** kan du se information liknande toohello följande bild:</span><span class="sxs-lookup"><span data-stu-id="25759-314">If you visit hello Oozie Web UI and select hello **Coordinator Jobs** tab, you see information similar toohello following image:</span></span>

    ![fliken för Coordinator jobb](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    <span data-ttu-id="25759-316">Hej **nästa materialisering** posten innehåller hello nästa gång hello jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="25759-316">hello **Next Materialization** entry contains hello next time that hello job runs.</span></span>

8. <span data-ttu-id="25759-317">Liknande toohello tidigare arbetsflödesjobbet Markera hello jobbet post i hello webbgränssnittet visar information om hello jobb:</span><span class="sxs-lookup"><span data-stu-id="25759-317">Similar toohello earlier workflow job, selecting hello job entry in hello web UI displays information on hello job:</span></span>

    ![Coordinator Jobbinformationen](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    > [!NOTE]
    > <span data-ttu-id="25759-319">Den här bilden visar endast lyckade körningar av hello jobb, inte enskilda åtgärder hello schemalagda arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="25759-319">This image only shows successful runs of hello job, not individual actions within hello scheduled workflow.</span></span> <span data-ttu-id="25759-320">toosee som väljer du något av hello **åtgärd** poster.</span><span class="sxs-lookup"><span data-stu-id="25759-320">toosee that, select one of hello **Action** entries.</span></span>

    ![Åtgärden info](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

## <a name="troubleshooting"></a><span data-ttu-id="25759-322">Felsökning</span><span class="sxs-lookup"><span data-stu-id="25759-322">Troubleshooting</span></span>

<span data-ttu-id="25759-323">Hej Oozie UI kan tooview Oozie loggar.</span><span class="sxs-lookup"><span data-stu-id="25759-323">hello Oozie UI allows you tooview Oozie logs.</span></span> <span data-ttu-id="25759-324">Den innehåller också länkar tooJobTracker loggar för MapReduce-aktiviteter som startas av hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="25759-324">It also contains links tooJobTracker logs for MapReduce tasks started by hello workflow.</span></span> <span data-ttu-id="25759-325">hello mönster för att felsöka bör vara:</span><span class="sxs-lookup"><span data-stu-id="25759-325">hello pattern for troubleshooting should be:</span></span>

1. <span data-ttu-id="25759-326">Visa hello jobb i Oozie-Webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="25759-326">View hello job in Oozie Web UI.</span></span>

2. <span data-ttu-id="25759-327">Om det finns ett fel eller ett misslyckande för en specifik åtgärd, Välj hello åtgärd toosee om hello **felmeddelande** fältet innehåller mer information om hello-fel.</span><span class="sxs-lookup"><span data-stu-id="25759-327">If there is an error or failure for a specific action, select hello action toosee if hello **Error Message** field provides more information on hello failure.</span></span>

3. <span data-ttu-id="25759-328">Om det finns använder du hello URL: en från hello åtgärd tooview mer information (till exempel JobTracker loggar) för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="25759-328">If available, use hello URL from hello action tooview more details (such as JobTracker logs) for hello action.</span></span>

<span data-ttu-id="25759-329">hello nedan visas specifika fel som kan uppstå, och hur tooresolve dem.</span><span class="sxs-lookup"><span data-stu-id="25759-329">hello following are specific errors you may encounter, and how tooresolve them.</span></span>

### <a name="ja009-cannot-initialize-cluster"></a><span data-ttu-id="25759-330">JA009: Det går inte att initiera kluster</span><span class="sxs-lookup"><span data-stu-id="25759-330">JA009: Cannot initialize cluster</span></span>

<span data-ttu-id="25759-331">**Symptom**: hello ändringar av jobbstatus för**PAUSAD**.</span><span class="sxs-lookup"><span data-stu-id="25759-331">**Symptoms**: hello job status changes too**SUSPENDED**.</span></span> <span data-ttu-id="25759-332">Information om hello jobbet visar hello RunHiveScript status som **START_MANUAL**.</span><span class="sxs-lookup"><span data-stu-id="25759-332">Details for hello job show hello RunHiveScript status as **START_MANUAL**.</span></span> <span data-ttu-id="25759-333">Välja hello åtgärd visar hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="25759-333">Selecting hello action displays hello following error message:</span></span>

    JA009: Cannot initialize Cluster. Please check your configuration for map

<span data-ttu-id="25759-334">**Orsak**: hello WASB-adresser som används i hello **job.xml** filen innehåller inte hello lagringsbehållaren eller namnet för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="25759-334">**Cause**: hello WASB addresses used in hello **job.xml** file do not contain hello storage container or storage account name.</span></span> <span data-ttu-id="25759-335">Hej WASB-adressformat måste vara `wasb://containername@storageaccountname.blob.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="25759-335">hello WASB address format must be `wasb://containername@storageaccountname.blob.core.windows.net`.</span></span>

<span data-ttu-id="25759-336">**Lösning**: ändra hello WASB-adresser som används av hello jobb.</span><span class="sxs-lookup"><span data-stu-id="25759-336">**Resolution**: Change hello WASB addresses used by hello job.</span></span>

### <a name="ja002-oozie-is-not-allowed-tooimpersonate-ltuser"></a><span data-ttu-id="25759-337">JA002: Oozie är inte tillåtet tooimpersonate &lt;användare ></span><span class="sxs-lookup"><span data-stu-id="25759-337">JA002: Oozie is not allowed tooimpersonate &lt;USER></span></span>

<span data-ttu-id="25759-338">**Symptom**: hello ändringar av jobbstatus för**PAUSAD**.</span><span class="sxs-lookup"><span data-stu-id="25759-338">**Symptoms**: hello job status changes too**SUSPENDED**.</span></span> <span data-ttu-id="25759-339">Information om hello jobbet visar hello RunHiveScript status som **START_MANUAL**.</span><span class="sxs-lookup"><span data-stu-id="25759-339">Details for hello job show hello RunHiveScript status as **START_MANUAL**.</span></span> <span data-ttu-id="25759-340">Välja hello åtgärd visar hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="25759-340">Selecting hello action shows hello following error message:</span></span>

    JA002: User: oozie is not allowed tooimpersonate <USER>

<span data-ttu-id="25759-341">**Orsak**: aktuella behörighetsinställningarna Tillåt inte Oozie tooimpersonate hello angivna användarkontot.</span><span class="sxs-lookup"><span data-stu-id="25759-341">**Cause**: Current permission settings do not allow Oozie tooimpersonate hello specified user account.</span></span>

<span data-ttu-id="25759-342">**Lösning**: Oozie tooimpersonate användare tillåts i hello **användare** grupp.</span><span class="sxs-lookup"><span data-stu-id="25759-342">**Resolution**: Oozie is allowed tooimpersonate users in hello **users** group.</span></span> <span data-ttu-id="25759-343">Använd hello `groups USERNAME` toosee hello grupper som hello användarkonto är medlem i.</span><span class="sxs-lookup"><span data-stu-id="25759-343">Use hello `groups USERNAME` toosee hello groups that hello user account is a member of.</span></span> <span data-ttu-id="25759-344">Om hello användaren inte är medlem i hello **användare** och använda hello efter kommandot tooadd hello toohello grupp:</span><span class="sxs-lookup"><span data-stu-id="25759-344">If hello user is not a member of hello **users** group, use hello following command tooadd hello user toohello group:</span></span>

    sudo adduser USERNAME users

> [!NOTE]
> <span data-ttu-id="25759-345">Det kan ta flera minuter innan HDInsight identifierar hello användaren har lagts till toohello grupp.</span><span class="sxs-lookup"><span data-stu-id="25759-345">It may take several minutes before HDInsight recognizes that hello user has been added toohello group.</span></span>

### <a name="launcher-error-sqoop"></a><span data-ttu-id="25759-346">Starta fel (Sqoop)</span><span class="sxs-lookup"><span data-stu-id="25759-346">Launcher ERROR (Sqoop)</span></span>

<span data-ttu-id="25759-347">**Symptom**: hello ändringar av jobbstatus för**KILLED**.</span><span class="sxs-lookup"><span data-stu-id="25759-347">**Symptoms**: hello job status changes too**KILLED**.</span></span> <span data-ttu-id="25759-348">Information om hello jobbet visar hello RunSqoopExport status som **fel**.</span><span class="sxs-lookup"><span data-stu-id="25759-348">Details for hello job show hello RunSqoopExport status as **ERROR**.</span></span> <span data-ttu-id="25759-349">Välja hello åtgärd visar hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="25759-349">Selecting hello action shows hello following error message:</span></span>

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

<span data-ttu-id="25759-350">**Orsak**: Sqoop är tooload hello databasen krävs tooaccess hello databas.</span><span class="sxs-lookup"><span data-stu-id="25759-350">**Cause**: Sqoop is unable tooload hello database driver required tooaccess hello database.</span></span>

<span data-ttu-id="25759-351">**Lösning**: när du använder Sqoop från ett Oozie-jobb, måste du inkludera hello databasdrivrutinen med hello andra resurser (till exempel hello workflow.xml) som används av hello jobb.</span><span class="sxs-lookup"><span data-stu-id="25759-351">**Resolution**: When using Sqoop from an Oozie job, you must include hello database driver with hello other resources (such as hello workflow.xml) used by hello job.</span></span> <span data-ttu-id="25759-352">Också referera hello Arkiv som innehåller hello databasdrivrutinen från hello `<sqoop>...</sqoop>` avsnitt i hello workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="25759-352">Also Reference hello archive containing hello database driver from hello `<sqoop>...</sqoop>` section of hello workflow.xml.</span></span>

<span data-ttu-id="25759-353">Till exempel använder hello-jobbet i det här dokumentet hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="25759-353">For example, for hello job in this document, you would use hello following steps:</span></span>

1. <span data-ttu-id="25759-354">Kopiera hello sqljdbc4.1.jar toohello /tutorials/useoozie katalog:</span><span class="sxs-lookup"><span data-stu-id="25759-354">Copy hello sqljdbc4.1.jar file toohello /tutorials/useoozie directory:</span></span>

    ```
    hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar
    ```

2. <span data-ttu-id="25759-355">Ändra följande XML på en ny rad ovanför hello workflow.xml tooadd hello `</sqoop>`:</span><span class="sxs-lookup"><span data-stu-id="25759-355">Modify hello workflow.xml tooadd hello following XML on a new line above `</sqoop>`:</span></span>

    ```xml
    <archive>sqljdbc41.jar</archive>
    ```

## <a name="next-steps"></a><span data-ttu-id="25759-356">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="25759-356">Next steps</span></span>

<span data-ttu-id="25759-357">I kursen får du lärt dig hur toodefine ett arbetsflöde för Oozie och hur toorun ett Oozie-jobb.</span><span class="sxs-lookup"><span data-stu-id="25759-357">In this tutorial, you learned how toodefine an Oozie workflow and how toorun an Oozie job.</span></span> <span data-ttu-id="25759-358">toolearn mer information om hur du arbetar med HDInsight finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="25759-358">toolearn more about working with HDInsight, see hello following articles:</span></span>

* <span data-ttu-id="25759-359">[Använd tidsbaserad Oozie-koordinator med HDInsight][hdinsight-oozie-coordinator-time]</span><span class="sxs-lookup"><span data-stu-id="25759-359">[Use time-based Oozie Coordinator with HDInsight][hdinsight-oozie-coordinator-time]</span></span>
* <span data-ttu-id="25759-360">[Överföra data för Hadoop-jobb i HDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="25759-360">[Upload data for Hadoop jobs in HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="25759-361">[Använda Sqoop med Hadoop i HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="25759-361">[Use Sqoop with Hadoop in HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="25759-362">[Använda Hive med Hadoop i HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="25759-362">[Use Hive with Hadoop on HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="25759-363">[Använda Pig med Hadoop i HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="25759-363">[Use Pig with Hadoop on HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="25759-364">[Utveckla Java-MapReduce-program för HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="25759-364">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563
[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started]: hdinsight-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started-emulator]: hdinsight-get-started-emulator.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: sql-database-create-configure.md
[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
