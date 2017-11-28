---
title: "aaaGet igång med exempel HBase på HDInsight - Azure | Microsoft Docs"
description: "Följ den här toostart för exempel av Apache HBase med hadoop i HDInsight. Skapa tabeller från hello HBase-gränssnittet och fråga dem med Hive."
keywords: hbasecommand,hbase example
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 4d6a2658-6b19-4268-95ee-822890f5a33a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 43419780142b320b16180a2b1f25020dee2f7a11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-an-apache-hbase-example-in-hdinsight"></a><span data-ttu-id="1e104-105">Kom igång med ett Apache HBase-exempel i HDInsight</span><span class="sxs-lookup"><span data-stu-id="1e104-105">Get started with an Apache HBase example in HDInsight</span></span>

<span data-ttu-id="1e104-106">Lär dig hur toocreate ett HBase-kluster i HDInsight, skapa HBase-tabeller och frågetabeller med Hive.</span><span class="sxs-lookup"><span data-stu-id="1e104-106">Learn how toocreate an HBase cluster in HDInsight, create HBase tables, and query tables by using Hive.</span></span> <span data-ttu-id="1e104-107">Allmän HBase-information finns i [HDInsight HBase-översikt][hdinsight-hbase-overview].</span><span class="sxs-lookup"><span data-stu-id="1e104-107">For general HBase information, see [HDInsight HBase overview][hdinsight-hbase-overview].</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a><span data-ttu-id="1e104-108">Krav</span><span class="sxs-lookup"><span data-stu-id="1e104-108">Prerequisites</span></span>
<span data-ttu-id="1e104-109">Innan du försöker exemplet HBase, måste du ha hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="1e104-109">Before you begin trying this HBase example, you must have hello following items:</span></span>

* <span data-ttu-id="1e104-110">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="1e104-110">**An Azure subscription**.</span></span> <span data-ttu-id="1e104-111">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="1e104-111">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="1e104-112">[Secure Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="1e104-112">[Secure Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> 
* <span data-ttu-id="1e104-113">[curl](http://curl.haxx.se/download.html).</span><span class="sxs-lookup"><span data-stu-id="1e104-113">[curl](http://curl.haxx.se/download.html).</span></span>

## <a name="create-hbase-cluster"></a><span data-ttu-id="1e104-114">Skapa HBase-kluster</span><span class="sxs-lookup"><span data-stu-id="1e104-114">Create HBase cluster</span></span>
<span data-ttu-id="1e104-115">hello följande procedur används en mall för Azure Resource Manager-toocreate en version 3.4 Linux-baserade HBase-kluster och hello beroende Azure standardkontot för lagring.</span><span class="sxs-lookup"><span data-stu-id="1e104-115">hello following procedure uses an Azure Resource Manager template toocreate a version 3.4 Linux-based HBase cluster and hello dependent default Azure Storage account.</span></span> <span data-ttu-id="1e104-116">toounderstand hello parametrar som används i hello proceduren och andra metoder, se [skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="1e104-116">toounderstand hello parameters used in hello procedure and other cluster creation methods, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="1e104-117">Klicka på hello följande bild tooopen hello mallen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1e104-117">Click hello following image tooopen hello template in hello Azure portal.</span></span> <span data-ttu-id="1e104-118">hello-mallen finns i en offentlig blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="1e104-118">hello template is located in a public blob container.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. <span data-ttu-id="1e104-119">Från hello **anpassad distribution** bladet ange hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="1e104-119">From hello **Custom deployment** blade, enter hello following values:</span></span>
   
   * <span data-ttu-id="1e104-120">**Prenumerationen**: Välj din Azure-prenumeration som används toocreate hello klustret.</span><span class="sxs-lookup"><span data-stu-id="1e104-120">**Subscription**: Select your Azure subscription that is used toocreate hello cluster.</span></span>
   * <span data-ttu-id="1e104-121">**Resursgrupp**: Skapa en Azure Resource Management-grupp eller använd en befintlig.</span><span class="sxs-lookup"><span data-stu-id="1e104-121">**Resource group**: Create an Azure Resource Management group or use an existing one.</span></span>
   * <span data-ttu-id="1e104-122">**Plats**: Ange hello plats hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="1e104-122">**Location**: Specify hello location of hello resource group.</span></span> 
   * <span data-ttu-id="1e104-123">**Klusternamn**: Ange ett namn för hello HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="1e104-123">**ClusterName**: Enter a name for hello HBase cluster.</span></span>
   * <span data-ttu-id="1e104-124">**Klustrets inloggningsnamn och lösenord**: hello standard inloggningsnamnet är **admin**.</span><span class="sxs-lookup"><span data-stu-id="1e104-124">**Cluster login name and password**: hello default login name is **admin**.</span></span>
   * <span data-ttu-id="1e104-125">**SSH-användarnamn och lösenord**: hello Standardanvändarnamnet är **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="1e104-125">**SSH username and password**: hello default username is **sshuser**.</span></span>  <span data-ttu-id="1e104-126">Du kan byta namn.</span><span class="sxs-lookup"><span data-stu-id="1e104-126">You can rename it.</span></span>
     
     <span data-ttu-id="1e104-127">Andra parametrar är valfria.</span><span class="sxs-lookup"><span data-stu-id="1e104-127">Other parameters are optional.</span></span>  
     
     <span data-ttu-id="1e104-128">Varje kluster är beroende av ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1e104-128">Each cluster has an Azure Storage account dependency.</span></span> <span data-ttu-id="1e104-129">När du har tagit bort ett kluster behåller hello data i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1e104-129">After you delete a cluster, hello data retains in hello storage account.</span></span> <span data-ttu-id="1e104-130">hello klustret standard lagringskontonamnet är hello klusternamnet med tillägget ”store”.</span><span class="sxs-lookup"><span data-stu-id="1e104-130">hello cluster default storage account name is hello cluster name with "store" appended.</span></span> <span data-ttu-id="1e104-131">Det är hårdkodat i hello mallsektion variabler.</span><span class="sxs-lookup"><span data-stu-id="1e104-131">It is hardcoded in hello template variables section.</span></span>
3. <span data-ttu-id="1e104-132">Välj **acceptera toohello villkoren ovan**, och klicka sedan på **inköp**.</span><span class="sxs-lookup"><span data-stu-id="1e104-132">Select **I agree toohello terms and conditions stated above**, and then click **Purchase**.</span></span> <span data-ttu-id="1e104-133">Det tar cirka 20 minuter toocreate ett kluster.</span><span class="sxs-lookup"><span data-stu-id="1e104-133">It takes about 20 minutes toocreate a cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="1e104-134">När ett HBase-kluster har tagits bort kan du skapa ett annat HBase-kluster med hjälp av hello samma standardblob-behållare.</span><span class="sxs-lookup"><span data-stu-id="1e104-134">After an HBase cluster is deleted, you can create another HBase cluster by using hello same default blob container.</span></span> <span data-ttu-id="1e104-135">hello nya klustret hämtar hello HBase-tabeller som du skapade i hello ursprungliga klustret.</span><span class="sxs-lookup"><span data-stu-id="1e104-135">hello new cluster picks up hello HBase tables you created in hello original cluster.</span></span> <span data-ttu-id="1e104-136">tooavoid inkonsekvenser, rekommenderar vi att du inaktiverar hello HBase-tabeller innan du tar bort hello klustret.</span><span class="sxs-lookup"><span data-stu-id="1e104-136">tooavoid inconsistencies, we recommend that you disable hello HBase tables before you delete hello cluster.</span></span>
> 
> 

## <a name="create-tables-and-insert-data"></a><span data-ttu-id="1e104-137">Skapa tabeller och infoga data</span><span class="sxs-lookup"><span data-stu-id="1e104-137">Create tables and insert data</span></span>
<span data-ttu-id="1e104-138">Du kan använda SSH tooconnect tooHBase kluster och sedan använda HBase Shell toocreate HBase-tabeller, infoga data och fråga efter data.</span><span class="sxs-lookup"><span data-stu-id="1e104-138">You can use SSH tooconnect tooHBase clusters and then use HBase Shell toocreate HBase tables, insert data, and query data.</span></span> <span data-ttu-id="1e104-139">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="1e104-139">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="1e104-140">För de flesta visas data i tabellformat hello:</span><span class="sxs-lookup"><span data-stu-id="1e104-140">For most people, data appears in hello tabular format:</span></span>

![HDInsight HBase-tabelldata][img-hbase-sample-data-tabular]

<span data-ttu-id="1e104-142">Hej samma data ser ut som i HBase (en implementering av BigTable):</span><span class="sxs-lookup"><span data-stu-id="1e104-142">In HBase (an implementation of BigTable), hello same data looks like:</span></span>

![HDInsight HBase BigTable-data][img-hbase-sample-data-bigtable]


<span data-ttu-id="1e104-144">**toouse hello HBase-gränssnittet**</span><span class="sxs-lookup"><span data-stu-id="1e104-144">**toouse hello HBase shell**</span></span>

1. <span data-ttu-id="1e104-145">Kör hello följande HBase-kommando från SSH:</span><span class="sxs-lookup"><span data-stu-id="1e104-145">From SSH, run hello following HBase command:</span></span>
   
    ```bash
    hbase shell
    ```

2. <span data-ttu-id="1e104-146">Skapa en HBase med två kolumnserier:</span><span class="sxs-lookup"><span data-stu-id="1e104-146">Create an HBase with two-column families:</span></span>

    ```hbaseshell   
    create 'Contacts', 'Personal', 'Office'
    list
    ```
3. <span data-ttu-id="1e104-147">Infoga data:</span><span class="sxs-lookup"><span data-stu-id="1e104-147">Insert some data:</span></span>
    
    ```hbaseshell   
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    scan 'Contacts'
    ```
   
    ![HDInsight Hadoop HBase shell][img-hbase-shell]
4. <span data-ttu-id="1e104-149">Hämta en rad</span><span class="sxs-lookup"><span data-stu-id="1e104-149">Get a single row</span></span>
   
    ```hbaseshell
    get 'Contacts', '1000'
    ```
   
    <span data-ttu-id="1e104-150">Hello samma resultat visas som med hello skanningskommandot eftersom det är bara en rad.</span><span class="sxs-lookup"><span data-stu-id="1e104-150">You shall see hello same results as using hello scan command because there is only one row.</span></span>
   
    <span data-ttu-id="1e104-151">Mer information om hello HBase-tabellschemat finns [introduktion tooHBase schemat Design][hbase-schema].</span><span class="sxs-lookup"><span data-stu-id="1e104-151">For more information about hello HBase table schema, see [Introduction tooHBase Schema Design][hbase-schema].</span></span> <span data-ttu-id="1e104-152">Fler HBase-kommandon finns i [referensguiden för Apache HBase][hbase-quick-start].</span><span class="sxs-lookup"><span data-stu-id="1e104-152">For more HBase commands, see [Apache HBase reference guide][hbase-quick-start].</span></span>
5. <span data-ttu-id="1e104-153">Avsluta hello shell</span><span class="sxs-lookup"><span data-stu-id="1e104-153">Exit hello shell</span></span>
   
    ```hbaseshell
    exit
    ```

<span data-ttu-id="1e104-154">**toobulk Läs in data i HBase-tabellen för hello kontakter**</span><span class="sxs-lookup"><span data-stu-id="1e104-154">**toobulk load data into hello contacts HBase table**</span></span>

<span data-ttu-id="1e104-155">HBase innehåller flera metoder för att läsa in data i tabeller.</span><span class="sxs-lookup"><span data-stu-id="1e104-155">HBase includes several methods of loading data into tables.</span></span>  <span data-ttu-id="1e104-156">Mer information finns i [Massinläsning](http://hbase.apache.org/book.html#arch.bulk.load).</span><span class="sxs-lookup"><span data-stu-id="1e104-156">For more information, see [Bulk loading](http://hbase.apache.org/book.html#arch.bulk.load).</span></span>

<span data-ttu-id="1e104-157">En exempeldatafil finns i en offentlig blob-behållare, *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.</span><span class="sxs-lookup"><span data-stu-id="1e104-157">A sample data file can be found in a public blob container, *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.</span></span>  <span data-ttu-id="1e104-158">hello är hello datafilen:</span><span class="sxs-lookup"><span data-stu-id="1e104-158">hello content of hello data file is:</span></span>

    8396    Calvin Raji      230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu         646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie         508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson     674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller    397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile     592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee        870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes       599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander  670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander    998-555-0171    230-555-0200    771 Northridge Drive

<span data-ttu-id="1e104-159">Du kan om du vill skapa en textfil och överföra hello filen tooyour egna storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1e104-159">You can optionally create a text file and upload hello file tooyour own storage account.</span></span> <span data-ttu-id="1e104-160">Hello instruktioner finns i [överföra data för Hadoop-jobb i HDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="1e104-160">For hello instructions, see [Upload data for Hadoop jobs in HDInsight][hdinsight-upload-data].</span></span>

> [!NOTE]
> <span data-ttu-id="1e104-161">Den här proceduren använder hello kontakter HBase-tabell som du har skapat i hello föregående procedur.</span><span class="sxs-lookup"><span data-stu-id="1e104-161">This procedure uses hello Contacts HBase table you have created in hello last procedure.</span></span>
> 

1. <span data-ttu-id="1e104-162">Kör följande kommando tootransform hello filen tooStoreFiles och lagra vid en relativ sökväg som anges av Dimporttsv.bulk.output hello från SSH.</span><span class="sxs-lookup"><span data-stu-id="1e104-162">From SSH, run hello following command tootransform hello data file tooStoreFiles and store at a relative path specified by Dimporttsv.bulk.output.</span></span>  <span data-ttu-id="1e104-163">Om du är i HBase Shell Använd hello avsluta kommandot tooexit.</span><span class="sxs-lookup"><span data-stu-id="1e104-163">If you are in HBase Shell, use hello exit command tooexit.</span></span>

    ```bash   
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
    ```

2. <span data-ttu-id="1e104-164">Kör följande kommando tooupload hello data från/storedatafileoutput toohello HBase-tabellen hello:</span><span class="sxs-lookup"><span data-stu-id="1e104-164">Run hello following command tooupload hello data from  /example/data/storeDataFileOutput toohello HBase table:</span></span>
   
    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
    ```

3. <span data-ttu-id="1e104-165">Du kan öppna hello HBase-gränssnittet och använda hello genomsökning kommandot toolist hello innehållet.</span><span class="sxs-lookup"><span data-stu-id="1e104-165">You can open hello HBase shell, and use hello scan command toolist hello table content.</span></span>

## <a name="use-hive-tooquery-hbase"></a><span data-ttu-id="1e104-166">Använda Hive tooquery HBase</span><span class="sxs-lookup"><span data-stu-id="1e104-166">Use Hive tooquery HBase</span></span>

<span data-ttu-id="1e104-167">Du kan fråga efter data i HBase-tabeller med hjälp av Hive.</span><span class="sxs-lookup"><span data-stu-id="1e104-167">You can query data in HBase tables by using Hive.</span></span> <span data-ttu-id="1e104-168">I det här avsnittet skapar du en Hive-tabell som mappar toohello HBase-tabellen och använder den tooquery hello data i HBase-tabellen.</span><span class="sxs-lookup"><span data-stu-id="1e104-168">In this section, you create a Hive table that maps toohello HBase table and uses it tooquery hello data in your HBase table.</span></span>

1. <span data-ttu-id="1e104-169">Öppna **PuTTY**, och Anslut toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="1e104-169">Open **PuTTY**, and connect toohello cluster.</span></span>  <span data-ttu-id="1e104-170">Se hello instruktioner i hello föregående procedur.</span><span class="sxs-lookup"><span data-stu-id="1e104-170">See hello instructions in hello previous procedure.</span></span>
2. <span data-ttu-id="1e104-171">Använd följande kommando toostart Beeline hello från hello SSH-sessionen:</span><span class="sxs-lookup"><span data-stu-id="1e104-171">From hello SSH session, use hello following command toostart Beeline:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    <span data-ttu-id="1e104-172">Mer information om Beeline finns i [Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md) (Använda Hive med Hadoop i HDInsight med Beeline).</span><span class="sxs-lookup"><span data-stu-id="1e104-172">For more information about Beeline, see [Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md).</span></span>
       
3. <span data-ttu-id="1e104-173">Kör följande HiveQL-skript toocreate hello en Hive-tabell som mappar toohello HBase-tabellen.</span><span class="sxs-lookup"><span data-stu-id="1e104-173">Run hello following HiveQL script  toocreate a Hive table that maps toohello HBase table.</span></span> <span data-ttu-id="1e104-174">Kontrollera att du har skapat hello exempeltabell hänvisade till tidigare i den här kursen med hjälp av hello HBase-gränssnittet innan du kör instruktionen.</span><span class="sxs-lookup"><span data-stu-id="1e104-174">Make sure that you have created hello sample table referenced earlier in this tutorial by using hello HBase shell before you run this statement.</span></span>

    ```hiveql   
    CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
    STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
    TBLPROPERTIES ('hbase.table.name' = 'Contacts');
    ```

4. <span data-ttu-id="1e104-175">Kör följande HiveQL-skript tooquery hello data i hello HBase-tabellen hello:</span><span class="sxs-lookup"><span data-stu-id="1e104-175">Run hello following HiveQL script tooquery hello data in hello HBase table:</span></span>

    ```hiveql   
    SELECT count(rowkey) FROM hbasecontacts;
    ```

## <a name="use-hbase-rest-apis-using-curl"></a><span data-ttu-id="1e104-176">Använd HBase REST API:er med Curl</span><span class="sxs-lookup"><span data-stu-id="1e104-176">Use HBase REST APIs using Curl</span></span>

<span data-ttu-id="1e104-177">hello REST API skyddas via [grundläggande autentisering](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="1e104-177">hello REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="1e104-178">Du bör alltid göra begäranden genom att använda säker HTTP (HTTPS) toohelp se till att dina autentiseringsuppgifter skickas toohello server på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="1e104-178">You shall always make requests by using Secure HTTP (HTTPS) toohelp ensure that your credentials are securely sent toohello server.</span></span>

2. <span data-ttu-id="1e104-179">Använd hello följande kommando toolist hello befintliga HBase-tabeller:</span><span class="sxs-lookup"><span data-stu-id="1e104-179">Use hello following command toolist hello existing HBase tables:</span></span>

    ```bash
    curl -u <UserName>:<Password> \
    -G https://<ClusterName>.azurehdinsight.net/hbaserest/
    ```

3. <span data-ttu-id="1e104-180">Använd följande kommando toocreate en ny HBase-tabell med två kolumnserier hello:</span><span class="sxs-lookup"><span data-stu-id="1e104-180">Use hello following command toocreate a new HBase table with two-column families:</span></span>

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
    -v
    ```

    <span data-ttu-id="1e104-181">hello schemat tillhandahålls i hello JSon-format.</span><span class="sxs-lookup"><span data-stu-id="1e104-181">hello schema is provided in hello JSon format.</span></span>
4. <span data-ttu-id="1e104-182">Använd följande kommando tooinsert hello vissa data:</span><span class="sxs-lookup"><span data-stu-id="1e104-182">Use hello following command tooinsert some data:</span></span>

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
    -v
    ```
   
    <span data-ttu-id="1e104-183">Du måste base64 koda hello värdena som anges i hello -d växel.</span><span class="sxs-lookup"><span data-stu-id="1e104-183">You must base64 encode hello values specified in hello -d switch.</span></span> <span data-ttu-id="1e104-184">I exemplet hello:</span><span class="sxs-lookup"><span data-stu-id="1e104-184">In hello example:</span></span>
   
   * <span data-ttu-id="1e104-185">MTAwMA ==: 1000</span><span class="sxs-lookup"><span data-stu-id="1e104-185">MTAwMA==: 1000</span></span>
   * <span data-ttu-id="1e104-186">UGVyc29uYWw6TmFtZQ==: Personal:Name</span><span class="sxs-lookup"><span data-stu-id="1e104-186">UGVyc29uYWw6TmFtZQ==: Personal:Name</span></span>
   * <span data-ttu-id="1e104-187">Sm9obiBEb2xl: John Dole</span><span class="sxs-lookup"><span data-stu-id="1e104-187">Sm9obiBEb2xl: John Dole</span></span>
     
     <span data-ttu-id="1e104-188">[FALSKT radnyckel](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) kan du tooinsert (batch) flera värden.</span><span class="sxs-lookup"><span data-stu-id="1e104-188">[false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) allows you tooinsert multiple (batched) values.</span></span>
5. <span data-ttu-id="1e104-189">Använd följande kommando tooget en rad hello:</span><span class="sxs-lookup"><span data-stu-id="1e104-189">Use hello following command tooget a row:</span></span>
   
    ```bash 
    curl -u <UserName>:<Password> \
    -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
    -H "Accept: application/json" \
    -v
    ```

<span data-ttu-id="1e104-190">Mer information om HBase Rest finns i [Referensguiden för Apache HBase](https://hbase.apache.org/book.html#_rest).</span><span class="sxs-lookup"><span data-stu-id="1e104-190">For more information about HBase Rest, see [Apache HBase Reference Guide](https://hbase.apache.org/book.html#_rest).</span></span>

> [!NOTE]
> <span data-ttu-id="1e104-191">Thrift stöds inte av HBase i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1e104-191">Thrift is not supported by HBase in HDInsight.</span></span>
>
> <span data-ttu-id="1e104-192">När du använder Curl eller annan REST-kommunikation med WebHCat, måste du autentisera hello begäranden genom att tillhandahålla hello användarnamn och lösenord för hello HDInsight-klustrets administratör.</span><span class="sxs-lookup"><span data-stu-id="1e104-192">When using Curl or any other REST communication with WebHCat, you must authenticate hello requests by providing hello user name and password for hello HDInsight cluster administrator.</span></span> <span data-ttu-id="1e104-193">Du måste också använda hello klustrets namn som en del av hello identifierare URI (Uniform Resource) används toosend hello begäranden toohello server:</span><span class="sxs-lookup"><span data-stu-id="1e104-193">You must also use hello cluster name as part of hello Uniform Resource Identifier (URI) used toosend hello requests toohello server:</span></span>
> 
>   
>        curl -u <UserName>:<Password> \
>        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
>   
>    <span data-ttu-id="1e104-194">Du bör få ett svar liknande toohello efter svar:</span><span class="sxs-lookup"><span data-stu-id="1e104-194">You should receive a response similar toohello following response:</span></span>
>   
>        {"status":"ok","version":"v1"}
   


## <a name="check-cluster-status"></a><span data-ttu-id="1e104-195">Kontrollera klusterstatus</span><span class="sxs-lookup"><span data-stu-id="1e104-195">Check cluster status</span></span>
<span data-ttu-id="1e104-196">HBase i HDInsight levereras med ett webbgränssnitt för övervakning av kluster.</span><span class="sxs-lookup"><span data-stu-id="1e104-196">HBase in HDInsight ships with a Web UI for monitoring clusters.</span></span> <span data-ttu-id="1e104-197">Du kan använda hello Webbgränssnittet för att begära statistik eller information om regioner.</span><span class="sxs-lookup"><span data-stu-id="1e104-197">Using hello Web UI, you can request statistics or information about regions.</span></span>

<span data-ttu-id="1e104-198">**tooaccess hello HBase Master UI**</span><span class="sxs-lookup"><span data-stu-id="1e104-198">**tooaccess hello HBase Master UI**</span></span>

1. <span data-ttu-id="1e104-199">Logga in på hello hello Ambari-Webbgränssnittet på https://&lt;klusternamn >. azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="1e104-199">Sign into hello hello Ambari Web UI at https://&lt;Clustername>.azurehdinsight.net.</span></span>
2. <span data-ttu-id="1e104-200">Klicka på **HBase** hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="1e104-200">Click **HBase** from hello left menu.</span></span>
3. <span data-ttu-id="1e104-201">Klicka på **snabblänkar** hello hello sida, punkt toohello active Zookeeper nod länk, och klicka sedan på **HBase Master UI**.</span><span class="sxs-lookup"><span data-stu-id="1e104-201">Click **Quick links** on hello top of hello page, point toohello active Zookeeper node link, and then click **HBase Master UI**.</span></span>  <span data-ttu-id="1e104-202">hello UI öppnas i en annan flik i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="1e104-202">hello UI is opened in another browser tab:</span></span>

  ![HDInsight HBase HMaster UI](./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

  <span data-ttu-id="1e104-204">Hej HBase Master UI innehåller hello följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="1e104-204">hello HBase Master UI contains hello following sections:</span></span>

  - <span data-ttu-id="1e104-205">regionservrar</span><span class="sxs-lookup"><span data-stu-id="1e104-205">region servers</span></span>
  - <span data-ttu-id="1e104-206">huvudservrar för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="1e104-206">backup masters</span></span>
  - <span data-ttu-id="1e104-207">tabeller</span><span class="sxs-lookup"><span data-stu-id="1e104-207">tables</span></span>
  - <span data-ttu-id="1e104-208">uppgifter</span><span class="sxs-lookup"><span data-stu-id="1e104-208">tasks</span></span>
  - <span data-ttu-id="1e104-209">attribut för programvara</span><span class="sxs-lookup"><span data-stu-id="1e104-209">software attributes</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="1e104-210">Ta bort hello kluster</span><span class="sxs-lookup"><span data-stu-id="1e104-210">Delete hello cluster</span></span>
<span data-ttu-id="1e104-211">tooavoid inkonsekvenser, rekommenderar vi att du inaktiverar hello HBase-tabeller innan du tar bort hello klustret.</span><span class="sxs-lookup"><span data-stu-id="1e104-211">tooavoid inconsistencies, we recommend that you disable hello HBase tables before you delete hello cluster.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="1e104-212">Felsöka</span><span class="sxs-lookup"><span data-stu-id="1e104-212">Troubleshoot</span></span>

<span data-ttu-id="1e104-213">Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="1e104-213">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e104-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e104-214">Next steps</span></span>
<span data-ttu-id="1e104-215">I den här artikeln får du lära dig hur hello toocreate ett HBase-kluster och hur toocreate tabellerna och vyerna hello data i dessa tabeller från HBase-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="1e104-215">In this article, you learned how toocreate an HBase cluster and how toocreate tables and view hello data in those tables from hello HBase shell.</span></span> <span data-ttu-id="1e104-216">Du också lära dig hur toouse registreringsdata fråga på data i HBase-tabeller och hur toouse hello HBase C# REST API: er toocreate en HBase-tabell och hämta data från hello tabell.</span><span class="sxs-lookup"><span data-stu-id="1e104-216">You also learned how toouse a Hive query on data in HBase tables and how toouse hello HBase C# REST APIs toocreate an HBase table and retrieve data from hello table.</span></span>

<span data-ttu-id="1e104-217">Det finns fler toolearn:</span><span class="sxs-lookup"><span data-stu-id="1e104-217">toolearn more, see:</span></span>

* <span data-ttu-id="1e104-218">[HDInsight HBase-översikt][hdinsight-hbase-overview]: HBase är en NoSQL-databas av Apachetyp med öppen källkod som bygger på Hadoop och som ger direktåtkomst och stark konsekvens för stora mängder ostrukturerade och semistrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="1e104-218">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
