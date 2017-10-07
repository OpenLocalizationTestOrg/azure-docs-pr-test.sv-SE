---
title: aaaApache Sqoop med Hadoop - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Apache Sqoop tooimport och exportera mellan Hadoop i HDInsight och en Azure SQL Database."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
author: Blackmist
tags: azure-portal
keywords: hadoop sqoop, sqoop
ms.assetid: 303649a5-4be5-4933-bf1d-4b232083c354
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: larryfr
ms.openlocfilehash: b256285659bbcf18ff05e220ccdf51c21eb8fbf7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-sqoop-tooimport-and-export-data-between-hadoop-on-hdinsight-and-sql-database"></a><span data-ttu-id="e9a83-104">Använd Apache Sqoop tooimport och exportera data mellan Hadoop i HDInsight och SQL-databas</span><span class="sxs-lookup"><span data-stu-id="e9a83-104">Use Apache Sqoop tooimport and export data between Hadoop on HDInsight and SQL Database</span></span>

[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="e9a83-105">Lär dig hur toouse Apache Sqoop tooimport och exportera mellan ett Hadoop-kluster i Azure HDInsight och Azure SQL Database eller Microsoft SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="e9a83-105">Learn how toouse Apache Sqoop tooimport and export between a Hadoop cluster in Azure HDInsight and Azure SQL Database or Microsoft SQL Server database.</span></span> <span data-ttu-id="e9a83-106">hello stegen i det här dokumentet används hello `sqoop` direkt från hello headnode av hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="e9a83-106">hello steps in this document use hello `sqoop` command directly from hello headnode of hello Hadoop cluster.</span></span> <span data-ttu-id="e9a83-107">Du använder SSH tooconnect toohello huvudnod och kör hello kommandon i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="e9a83-107">You use SSH tooconnect toohello head node and run hello commands in this document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e9a83-108">hello fungerar stegen i det här dokumentet endast med HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="e9a83-108">hello steps in this document only work with HDInsight clusters that use Linux.</span></span> <span data-ttu-id="e9a83-109">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e9a83-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="e9a83-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="e9a83-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="install-freetds"></a><span data-ttu-id="e9a83-111">Installera FreeTDS</span><span class="sxs-lookup"><span data-stu-id="e9a83-111">Install FreeTDS</span></span>

1. <span data-ttu-id="e9a83-112">Använda SSH tooconnect toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e9a83-112">Use SSH tooconnect toohello HDInsight cluster.</span></span> <span data-ttu-id="e9a83-113">Till exempel följande kommando hello ansluter toohello primära headnode i ett kluster med namnet `mycluster`:</span><span class="sxs-lookup"><span data-stu-id="e9a83-113">For example, hello following command connects toohello primary headnode of a cluster named `mycluster`:</span></span>

    ```bash
    ssh CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="e9a83-114">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="e9a83-114">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="e9a83-115">Använd följande kommando tooinstall FreeTDS hello:</span><span class="sxs-lookup"><span data-stu-id="e9a83-115">Use hello following command tooinstall FreeTDS:</span></span>

    ```bash
    sudo apt --assume-yes install freetds-dev freetds-bin
    ```

    <span data-ttu-id="e9a83-116">FreeTDS används i flera steg tooconnect tooSQL databas.</span><span class="sxs-lookup"><span data-stu-id="e9a83-116">FreeTDS is used in several steps tooconnect tooSQL Database.</span></span>

## <a name="create-hello-table-in-sql-database"></a><span data-ttu-id="e9a83-117">Skapa hello tabell i SQL-databas</span><span class="sxs-lookup"><span data-stu-id="e9a83-117">Create hello table in SQL Database</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e9a83-118">Om du använder hello HDInsight-kluster och SQL-databas som skapats i [skapa klustret och SQL database](hdinsight-use-sqoop.md), hoppa över hello steg i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="e9a83-118">If you are using hello HDInsight cluster and SQL Database created in [Create cluster and SQL database](hdinsight-use-sqoop.md), skip hello steps in this section.</span></span> <span data-ttu-id="e9a83-119">hello databas och tabell skapats som en del av hello stegen i hello [skapa klustret och SQL database](hdinsight-use-sqoop.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="e9a83-119">hello database and table were created as part of hello steps in hello [Create cluster and SQL database](hdinsight-use-sqoop.md) document.</span></span>

1. <span data-ttu-id="e9a83-120">Använd hello efter kommandot tooconnect toohello SQL Database-server från hello SSH-sessionen.</span><span class="sxs-lookup"><span data-stu-id="e9a83-120">From hello SSH session, use hello following command tooconnect toohello SQL Database server.</span></span>

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    <span data-ttu-id="e9a83-121">Du får utdata liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="e9a83-121">You receive output similar toohello following text:</span></span>

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set toosqooptest
        1>

2. <span data-ttu-id="e9a83-122">Vid hello `1>` uppmanar, ange hello följande fråga:</span><span class="sxs-lookup"><span data-stu-id="e9a83-122">At hello `1>` prompt, enter hello following query:</span></span>

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
    GO
    ```

    <span data-ttu-id="e9a83-123">När hello `GO` uttryck har angetts, hello tidigare rapporter utvärderas.</span><span class="sxs-lookup"><span data-stu-id="e9a83-123">When hello `GO` statement is entered, hello previous statements are evaluated.</span></span> <span data-ttu-id="e9a83-124">Först hello **mobiledata** tabell skapas och sedan ett grupperat index läggs tooit (obligatoriskt för SQL-databas).</span><span class="sxs-lookup"><span data-stu-id="e9a83-124">First, hello **mobiledata** table is created, then a clustered index is added tooit (required by SQL Database.)</span></span>

    <span data-ttu-id="e9a83-125">Använd hello följande fråga tooverify som hello tabellen har skapats:</span><span class="sxs-lookup"><span data-stu-id="e9a83-125">Use hello following query tooverify that hello table has been created:</span></span>

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="e9a83-126">Du kan se utdata liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="e9a83-126">You see output similar toohello following text:</span></span>

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

3. <span data-ttu-id="e9a83-127">Ange `exit` på hello `1>` fråga tooexit hello tsql-verktyget.</span><span class="sxs-lookup"><span data-stu-id="e9a83-127">Enter `exit` at hello `1>` prompt tooexit hello tsql utility.</span></span>

## <a name="sqoop-export"></a><span data-ttu-id="e9a83-128">Sqoop export</span><span class="sxs-lookup"><span data-stu-id="e9a83-128">Sqoop export</span></span>

1. <span data-ttu-id="e9a83-129">Använd hello följande kommando från hello SSH-anslutning toohello kluster tooverify att Sqoop kan se din SQL-databas:</span><span class="sxs-lookup"><span data-stu-id="e9a83-129">From hello SSH connection toohello cluster, use hello following command tooverify that Sqoop can see your SQL Database:</span></span>

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    <span data-ttu-id="e9a83-130">När du uppmanas du ange hello lösenord för hello inloggningen för SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="e9a83-130">When prompted, enter hello password for hello SQL Database login.</span></span>

    <span data-ttu-id="e9a83-131">Det här kommandot returnerar en lista över databaser, inklusive hello **sqooptest** databasen som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="e9a83-131">This command returns a list of databases, including hello **sqooptest** database that you created earlier.</span></span>

2. <span data-ttu-id="e9a83-132">tooexport data från **hivesampletable** toohello **mobiledata** tabell använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e9a83-132">tooexport data from **hivesampletable** toohello **mobiledata** table, use hello following command:</span></span>

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --export-dir 'wasb:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1
    ```

    <span data-ttu-id="e9a83-133">Det här kommandot instruerar Sqoop tooconnect toohello **sqooptest** databas.</span><span class="sxs-lookup"><span data-stu-id="e9a83-133">This command instructs Sqoop tooconnect toohello **sqooptest** database.</span></span> <span data-ttu-id="e9a83-134">Sqoop sedan exporterar data från **wasb: / / / hive/datalager/hivesampletable** toohello **mobiledata** tabell.</span><span class="sxs-lookup"><span data-stu-id="e9a83-134">Sqoop then exports data from **wasb:///hive/warehouse/hivesampletable** toohello **mobiledata** table.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="e9a83-135">Använd `wasb:///` om hello standardlagring för klustret är ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e9a83-135">Use `wasb:///` if hello default storage for your cluster is an Azure Storage account.</span></span> <span data-ttu-id="e9a83-136">Använd `adl:///` om det är ett Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="e9a83-136">Use `adl:///` if it is an Azure Data Lake Store.</span></span>

3. <span data-ttu-id="e9a83-137">När hello-kommandot har slutförts kan du använda följande kommando tooconnect toohello databasen med TSQL hello:</span><span class="sxs-lookup"><span data-stu-id="e9a83-137">After hello command completes, use hello following command tooconnect toohello database using TSQL:</span></span>

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P -p 1433 -D sqooptest
    ```

    <span data-ttu-id="e9a83-138">När du är ansluten, Använd hello följande instruktioner tooverify som hello data var exporterade toohello **mobiledata** tabell:</span><span class="sxs-lookup"><span data-stu-id="e9a83-138">Once connected, use hello following statements tooverify that hello data was exported toohello **mobiledata** table:</span></span>

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata
    GO
    ```

    <span data-ttu-id="e9a83-139">Du bör se en lista över data i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="e9a83-139">You should see a listing of data in hello table.</span></span> <span data-ttu-id="e9a83-140">Typen `exit` tooexit hello tsql-verktyget.</span><span class="sxs-lookup"><span data-stu-id="e9a83-140">Type `exit` tooexit hello tsql utility.</span></span>

## <a name="sqoop-import"></a><span data-ttu-id="e9a83-141">Sqoop import</span><span class="sxs-lookup"><span data-stu-id="e9a83-141">Sqoop import</span></span>

1. <span data-ttu-id="e9a83-142">Använd hello följande kommando tooimport data från hello **mobiledata** tabell i SQL-databas, toohello **wasb: / / / självstudier/usesqoop/importeddata** på HDInsight:</span><span class="sxs-lookup"><span data-stu-id="e9a83-142">Use hello following command tooimport data from hello **mobiledata** table in SQL Database, toohello **wasb:///tutorials/usesqoop/importeddata** directory on HDInsight:</span></span>

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    <span data-ttu-id="e9a83-143">hello fälten i hello data avgränsas med ett tabbtecken och hello rader avslutas med ett tecken för ny rad.</span><span class="sxs-lookup"><span data-stu-id="e9a83-143">hello fields in hello data are separated by a tab character, and hello lines are terminated by a new-line character.</span></span>

2. <span data-ttu-id="e9a83-144">När hello importen har slutförts använder du hello efter kommandot toolist ut hello data i hello ny katalog:</span><span class="sxs-lookup"><span data-stu-id="e9a83-144">Once hello import has completed, use hello following command toolist out hello data in hello new directory:</span></span>

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a><span data-ttu-id="e9a83-145">Använda SQLServer</span><span class="sxs-lookup"><span data-stu-id="e9a83-145">Using SQL Server</span></span>

<span data-ttu-id="e9a83-146">Du kan också använda Sqoop tooimport och exportera data från SQL Server i ditt datacenter eller på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="e9a83-146">You can also use Sqoop tooimport and export data from SQL Server, either in your data center or on a Virtual Machine hosted in Azure.</span></span> <span data-ttu-id="e9a83-147">hello skillnaderna mellan att använda SQL Database och SQL Server är:</span><span class="sxs-lookup"><span data-stu-id="e9a83-147">hello differences between using SQL Database and SQL Server are:</span></span>

* <span data-ttu-id="e9a83-148">HDInsight- och SQL Server måste vara på hello samma virtuella Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="e9a83-148">Both HDInsight and SQL Server must be on hello same Azure Virtual Network.</span></span>

    <span data-ttu-id="e9a83-149">Ett exempel finns hello [ansluta HDInsight tooyour lokalt nätverk](./connect-on-premises-network.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="e9a83-149">For an example, see hello [Connect HDInsight tooyour on-premises network](./connect-on-premises-network.md) document.</span></span>

    <span data-ttu-id="e9a83-150">Mer information om hur du använder HDInsight med ett Azure Virtual Network finns hello [utöka HDInsight med Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="e9a83-150">For more information on using HDInsight with an Azure Virtual Network, see hello [Extend HDInsight with Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span> <span data-ttu-id="e9a83-151">Mer information i Azure Virtual Network finns hello [översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="e9a83-151">For more information on Azure Virtual Network, see hello [Virtual Network Overview](../virtual-network/virtual-networks-overview.md) document.</span></span>

* <span data-ttu-id="e9a83-152">SQL Server måste vara konfigurerade tooallow SQL-autentisering.</span><span class="sxs-lookup"><span data-stu-id="e9a83-152">SQL Server must be configured tooallow SQL authentication.</span></span> <span data-ttu-id="e9a83-153">Mer information finns i hello [välja ett autentiseringsläge](https://msdn.microsoft.com/ms144284.aspx) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="e9a83-153">For more information, see hello [Choose an Authentication Mode](https://msdn.microsoft.com/ms144284.aspx) document.</span></span>

* <span data-ttu-id="e9a83-154">Du kan ha SQL Server tooconfigure tooaccept fjärranslutningar.</span><span class="sxs-lookup"><span data-stu-id="e9a83-154">You may have tooconfigure SQL Server tooaccept remote connections.</span></span> <span data-ttu-id="e9a83-155">Mer information finns i hello [hur tootroubleshoot anslutande toohello SQL Server database engine](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="e9a83-155">For more information, see hello [How tootroubleshoot connecting toohello SQL Server database engine](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) document.</span></span>

* <span data-ttu-id="e9a83-156">Skapa hello **sqooptest** databas i SQL Server med hjälp av ett verktyg som till exempel **SQL Server Management Studio** eller **tsql**.</span><span class="sxs-lookup"><span data-stu-id="e9a83-156">Create hello **sqooptest** database in SQL Server using a utility such as **SQL Server Management Studio** or **tsql**.</span></span> <span data-ttu-id="e9a83-157">hello steg för att använda hello Azure CLI fungerar bara för Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="e9a83-157">hello steps for using hello Azure CLI only work for Azure SQL Database.</span></span>

    <span data-ttu-id="e9a83-158">Använd hello följande Transact-SQL-instruktioner toocreate hello **mobiledata** tabell:</span><span class="sxs-lookup"><span data-stu-id="e9a83-158">Use hello following Transact-SQL statements toocreate hello **mobiledata** table:</span></span>

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    ```

* <span data-ttu-id="e9a83-159">När du ansluter toohello SQL Server från HDInsight kan kanske du toouse hello IP-adressen för hello SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e9a83-159">When connecting toohello SQL Server from HDInsight, you may have toouse hello IP address of hello SQL Server.</span></span> <span data-ttu-id="e9a83-160">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e9a83-160">For example:</span></span>

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a><span data-ttu-id="e9a83-161">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="e9a83-161">Limitations</span></span>

* <span data-ttu-id="e9a83-162">Masskapat export - med Linux-baserat HDInsight, hello Sqoop koppling som används tooexport data tooMicrosoft SQL Server eller Azure SQL Database stöder för närvarande inte bulkinfogningar.</span><span class="sxs-lookup"><span data-stu-id="e9a83-162">Bulk export - With Linux-based HDInsight, hello Sqoop connector used tooexport data tooMicrosoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>

* <span data-ttu-id="e9a83-163">Batchbearbetning - med Linux-baserat HDInsight när du använder hello `-batch` växeln när du utför infogningar, Sqoop gör flera infogningar i stället för batchbearbetning hello infogningsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="e9a83-163">Batching - With Linux-based HDInsight, When using hello `-batch` switch when performing inserts, Sqoop makes multiple inserts instead of batching hello insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e9a83-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e9a83-164">Next steps</span></span>

<span data-ttu-id="e9a83-165">Nu har du fått veta hur toouse Sqoop.</span><span class="sxs-lookup"><span data-stu-id="e9a83-165">Now you have learned how toouse Sqoop.</span></span> <span data-ttu-id="e9a83-166">Det finns fler toolearn:</span><span class="sxs-lookup"><span data-stu-id="e9a83-166">toolearn more, see:</span></span>

* <span data-ttu-id="e9a83-167">[Använda Oozie med HDInsight][hdinsight-use-oozie]: Använd Sqoop åtgärd i ett arbetsflöde för Oozie.</span><span class="sxs-lookup"><span data-stu-id="e9a83-167">[Use Oozie with HDInsight][hdinsight-use-oozie]: Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="e9a83-168">[Analysera svarta fördröjning data med HDInsight][hdinsight-analyze-flight-data]: använda Hive tooanalyze svarta fördröjning data och sedan använda Sqoop tooexport data tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="e9a83-168">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]: Use Hive tooanalyze flight delay data, and then use Sqoop tooexport data tooan Azure SQL database.</span></span>
* <span data-ttu-id="e9a83-169">[Ladda upp data tooHDInsight][hdinsight-upload-data]: hitta andra metoder för att överföra data tooHDInsight/Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="e9a83-169">[Upload data tooHDInsight][hdinsight-upload-data]: Find other methods for uploading data tooHDInsight/Azure Blob storage.</span></span>

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
