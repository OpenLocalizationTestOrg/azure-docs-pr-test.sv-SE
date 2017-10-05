---
title: "Kom igång med HBase-exempel på HDInsight - Azure | Microsoft Docs"
description: "Följ stegen i det här exemplet om Apache HBase om du vill börja använda hadoop med HDInsight. Skapa tabeller från HBase-gränssnittet och ställ frågor för dem med Hive."
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
ms.openlocfilehash: bbd8a838062795ee03ae02dc5e3fd45d841a6e17
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-an-apache-hbase-example-in-hdinsight"></a><span data-ttu-id="ce41f-105">Kom igång med ett Apache HBase-exempel i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ce41f-105">Get started with an Apache HBase example in HDInsight</span></span>

<span data-ttu-id="ce41f-106">Lär dig skapa ett HBase-kluster i HDInsight, skapa HBase-tabeller och frågetabeller med Hive.</span><span class="sxs-lookup"><span data-stu-id="ce41f-106">Learn how to create an HBase cluster in HDInsight, create HBase tables, and query tables by using Hive.</span></span> <span data-ttu-id="ce41f-107">Allmän HBase-information finns i [HDInsight HBase-översikt][hdinsight-hbase-overview].</span><span class="sxs-lookup"><span data-stu-id="ce41f-107">For general HBase information, see [HDInsight HBase overview][hdinsight-hbase-overview].</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a><span data-ttu-id="ce41f-108">Krav</span><span class="sxs-lookup"><span data-stu-id="ce41f-108">Prerequisites</span></span>
<span data-ttu-id="ce41f-109">Innan du testar det här HBase-exemplet måste du ha följande objekt:</span><span class="sxs-lookup"><span data-stu-id="ce41f-109">Before you begin trying this HBase example, you must have the following items:</span></span>

* <span data-ttu-id="ce41f-110">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="ce41f-110">**An Azure subscription**.</span></span> <span data-ttu-id="ce41f-111">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="ce41f-111">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="ce41f-112">[Secure Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ce41f-112">[Secure Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> 
* <span data-ttu-id="ce41f-113">[curl](http://curl.haxx.se/download.html).</span><span class="sxs-lookup"><span data-stu-id="ce41f-113">[curl](http://curl.haxx.se/download.html).</span></span>

## <a name="create-hbase-cluster"></a><span data-ttu-id="ce41f-114">Skapa HBase-kluster</span><span class="sxs-lookup"><span data-stu-id="ce41f-114">Create HBase cluster</span></span>
<span data-ttu-id="ce41f-115">Följande procedur använder en Azure Resource Manager-mall för att skapa ett version 3.4 Linux-baserat HBase-kluster och det beroende standardkontot för Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="ce41f-115">The following procedure uses an Azure Resource Manager template to create a version 3.4 Linux-based HBase cluster and the dependent default Azure Storage account.</span></span> <span data-ttu-id="ce41f-116">Mer information om de parametrar som används i proceduren och andra metoder för att skapa kluster finns i [Skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="ce41f-116">To understand the parameters used in the procedure and other cluster creation methods, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="ce41f-117">Klicka på följande bild för att öppna mallen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ce41f-117">Click the following image to open the template in the Azure portal.</span></span> <span data-ttu-id="ce41f-118">Mallen finns i en offentlig blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="ce41f-118">The template is located in a public blob container.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. <span data-ttu-id="ce41f-119">Från bladet **Anpassad distribution** anger du följande värden:</span><span class="sxs-lookup"><span data-stu-id="ce41f-119">From the **Custom deployment** blade, enter the following values:</span></span>
   
   * <span data-ttu-id="ce41f-120">**Prenumeration**: Välj den Azure-prenumeration som kommer användas för att skapa klustret.</span><span class="sxs-lookup"><span data-stu-id="ce41f-120">**Subscription**: Select your Azure subscription that is used to create the cluster.</span></span>
   * <span data-ttu-id="ce41f-121">**Resursgrupp**: Skapa en Azure Resource Management-grupp eller använd en befintlig.</span><span class="sxs-lookup"><span data-stu-id="ce41f-121">**Resource group**: Create an Azure Resource Management group or use an existing one.</span></span>
   * <span data-ttu-id="ce41f-122">**Plats**: Ange platsen för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="ce41f-122">**Location**: Specify the location of the resource group.</span></span> 
   * <span data-ttu-id="ce41f-123">**Klusternamn**: Ange ett namn för HBase-klustret.</span><span class="sxs-lookup"><span data-stu-id="ce41f-123">**ClusterName**: Enter a name for the HBase cluster.</span></span>
   * <span data-ttu-id="ce41f-124">**Klustrets inloggningsnamn och lösenord**: Inloggningsnamnet är som standard **admin**.</span><span class="sxs-lookup"><span data-stu-id="ce41f-124">**Cluster login name and password**: The default login name is **admin**.</span></span>
   * <span data-ttu-id="ce41f-125">**SSH-användarnamn och lösenord**: Standardanvändarnamnet är **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="ce41f-125">**SSH username and password**: The default username is **sshuser**.</span></span>  <span data-ttu-id="ce41f-126">Du kan byta namn.</span><span class="sxs-lookup"><span data-stu-id="ce41f-126">You can rename it.</span></span>
     
     <span data-ttu-id="ce41f-127">Andra parametrar är valfria.</span><span class="sxs-lookup"><span data-stu-id="ce41f-127">Other parameters are optional.</span></span>  
     
     <span data-ttu-id="ce41f-128">Varje kluster är beroende av ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ce41f-128">Each cluster has an Azure Storage account dependency.</span></span> <span data-ttu-id="ce41f-129">När du tar bort ett kluster stannar aktuella data kvar på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="ce41f-129">After you delete a cluster, the data retains in the storage account.</span></span> <span data-ttu-id="ce41f-130">Klustrets lagringskonto av standardtyp har det klusternamn som omfattar tillägget ”store”.</span><span class="sxs-lookup"><span data-stu-id="ce41f-130">The cluster default storage account name is the cluster name with "store" appended.</span></span> <span data-ttu-id="ce41f-131">Det är hårdkodat i avsnittet för mallvariabler.</span><span class="sxs-lookup"><span data-stu-id="ce41f-131">It is hardcoded in the template variables section.</span></span>
3. <span data-ttu-id="ce41f-132">Välj **Jag godkänner villkoren som anges ovan** och klicka sedan på **Köp**.</span><span class="sxs-lookup"><span data-stu-id="ce41f-132">Select **I agree to the terms and conditions stated above**, and then click **Purchase**.</span></span> <span data-ttu-id="ce41f-133">Det tar cirka 20 minuter att skapa ett kluster.</span><span class="sxs-lookup"><span data-stu-id="ce41f-133">It takes about 20 minutes to create a cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="ce41f-134">När ett HBase-kluster har tagits bort kan du skapa ett annat HBase-kluster med hjälp av samma standard-blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="ce41f-134">After an HBase cluster is deleted, you can create another HBase cluster by using the same default blob container.</span></span> <span data-ttu-id="ce41f-135">Det nya klustret hämtar de HBase-tabeller som du skapade i det ursprungliga klustret.</span><span class="sxs-lookup"><span data-stu-id="ce41f-135">The new cluster picks up the HBase tables you created in the original cluster.</span></span> <span data-ttu-id="ce41f-136">Om du vill undvika inkonsekvenser rekommenderar vi att du inaktiverar HBase-tabellerna innan du tar bort klustret.</span><span class="sxs-lookup"><span data-stu-id="ce41f-136">To avoid inconsistencies, we recommend that you disable the HBase tables before you delete the cluster.</span></span>
> 
> 

## <a name="create-tables-and-insert-data"></a><span data-ttu-id="ce41f-137">Skapa tabeller och infoga data</span><span class="sxs-lookup"><span data-stu-id="ce41f-137">Create tables and insert data</span></span>
<span data-ttu-id="ce41f-138">Du kan använda SSH för att ansluta till HBase-kluster och sedan använda HBase Shell för att skapa HBase-tabeller, infoga data och ställa frågor till data.</span><span class="sxs-lookup"><span data-stu-id="ce41f-138">You can use SSH to connect to HBase clusters and then use HBase Shell to create HBase tables, insert data, and query data.</span></span> <span data-ttu-id="ce41f-139">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="ce41f-139">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="ce41f-140">För de flesta visas data i tabellformat:</span><span class="sxs-lookup"><span data-stu-id="ce41f-140">For most people, data appears in the tabular format:</span></span>

![HDInsight HBase-tabelldata][img-hbase-sample-data-tabular]

<span data-ttu-id="ce41f-142">I HBase, som är en implementering av BigTable, ser samma data ut så här:</span><span class="sxs-lookup"><span data-stu-id="ce41f-142">In HBase (an implementation of BigTable), the same data looks like:</span></span>

![HDInsight HBase BigTable-data][img-hbase-sample-data-bigtable]


<span data-ttu-id="ce41f-144">**Använd HBase Shell**</span><span class="sxs-lookup"><span data-stu-id="ce41f-144">**To use the HBase shell**</span></span>

1. <span data-ttu-id="ce41f-145">Från SSH kör du följande HBase-kommando:</span><span class="sxs-lookup"><span data-stu-id="ce41f-145">From SSH, run the following HBase command:</span></span>
   
    ```bash
    hbase shell
    ```

2. <span data-ttu-id="ce41f-146">Skapa en HBase med två kolumnserier:</span><span class="sxs-lookup"><span data-stu-id="ce41f-146">Create an HBase with two-column families:</span></span>

    ```hbaseshell   
    create 'Contacts', 'Personal', 'Office'
    list
    ```
3. <span data-ttu-id="ce41f-147">Infoga data:</span><span class="sxs-lookup"><span data-stu-id="ce41f-147">Insert some data:</span></span>
    
    ```hbaseshell   
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    scan 'Contacts'
    ```
   
    ![HDInsight Hadoop HBase shell][img-hbase-shell]
4. <span data-ttu-id="ce41f-149">Hämta en rad</span><span class="sxs-lookup"><span data-stu-id="ce41f-149">Get a single row</span></span>
   
    ```hbaseshell
    get 'Contacts', '1000'
    ```
   
    <span data-ttu-id="ce41f-150">Samma resultat visas som med skanningskommandot eftersom det bara finns en rad.</span><span class="sxs-lookup"><span data-stu-id="ce41f-150">You shall see the same results as using the scan command because there is only one row.</span></span>
   
    <span data-ttu-id="ce41f-151">Mer information om HBase-tabellschemat finns i [Introduktion till design av HBase-schema][hbase-schema].</span><span class="sxs-lookup"><span data-stu-id="ce41f-151">For more information about the HBase table schema, see [Introduction to HBase Schema Design][hbase-schema].</span></span> <span data-ttu-id="ce41f-152">Fler HBase-kommandon finns i [referensguiden för Apache HBase][hbase-quick-start].</span><span class="sxs-lookup"><span data-stu-id="ce41f-152">For more HBase commands, see [Apache HBase reference guide][hbase-quick-start].</span></span>
5. <span data-ttu-id="ce41f-153">Lämna gränssnittet</span><span class="sxs-lookup"><span data-stu-id="ce41f-153">Exit the shell</span></span>
   
    ```hbaseshell
    exit
    ```

<span data-ttu-id="ce41f-154">**För att läsa in data i bulk i HBase-tabellen kontakter**</span><span class="sxs-lookup"><span data-stu-id="ce41f-154">**To bulk load data into the contacts HBase table**</span></span>

<span data-ttu-id="ce41f-155">HBase innehåller flera metoder för att läsa in data i tabeller.</span><span class="sxs-lookup"><span data-stu-id="ce41f-155">HBase includes several methods of loading data into tables.</span></span>  <span data-ttu-id="ce41f-156">Mer information finns i [Massinläsning](http://hbase.apache.org/book.html#arch.bulk.load).</span><span class="sxs-lookup"><span data-stu-id="ce41f-156">For more information, see [Bulk loading](http://hbase.apache.org/book.html#arch.bulk.load).</span></span>

<span data-ttu-id="ce41f-157">En exempeldatafil finns i en offentlig blob-behållare, *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.</span><span class="sxs-lookup"><span data-stu-id="ce41f-157">A sample data file can be found in a public blob container, *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.</span></span>  <span data-ttu-id="ce41f-158">Innehållet i datafilen är:</span><span class="sxs-lookup"><span data-stu-id="ce41f-158">The content of the data file is:</span></span>

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

<span data-ttu-id="ce41f-159">Du kan även skapa en textfil och överföra filen till ditt eget lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ce41f-159">You can optionally create a text file and upload the file to your own storage account.</span></span> <span data-ttu-id="ce41f-160">Anvisningarna finns i [Överföra data för Hadoop-jobb i HDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="ce41f-160">For the instructions, see [Upload data for Hadoop jobs in HDInsight][hdinsight-upload-data].</span></span>

> [!NOTE]
> <span data-ttu-id="ce41f-161">Den här proceduren använder den HBase-tabell för kontakter som du skapade i föregående procedur.</span><span class="sxs-lookup"><span data-stu-id="ce41f-161">This procedure uses the Contacts HBase table you have created in the last procedure.</span></span>
> 

1. <span data-ttu-id="ce41f-162">Kör följande kommando från SSH för att omvandla datafilen till StoreFiles och lagra vid en relativ sökväg som anges av Dimporttsv.bulk.output.</span><span class="sxs-lookup"><span data-stu-id="ce41f-162">From SSH, run the following command to transform the data file to StoreFiles and store at a relative path specified by Dimporttsv.bulk.output.</span></span>  <span data-ttu-id="ce41f-163">Om du är i HBase Shell använder du avslutningskommandot för att avsluta.</span><span class="sxs-lookup"><span data-stu-id="ce41f-163">If you are in HBase Shell, use the exit command to exit.</span></span>

    ```bash   
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
    ```

2. <span data-ttu-id="ce41f-164">Kör följande kommando för att överföra data från /example/data/storeDataFileOutput till  HBase-tabellen:</span><span class="sxs-lookup"><span data-stu-id="ce41f-164">Run the following command to upload the data from  /example/data/storeDataFileOutput to the HBase table:</span></span>
   
    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
    ```

3. <span data-ttu-id="ce41f-165">Du kan öppna HBase-gränssnittet och använda skanningskommandot för att lista tabellinnehållet.</span><span class="sxs-lookup"><span data-stu-id="ce41f-165">You can open the HBase shell, and use the scan command to list the table content.</span></span>

## <a name="use-hive-to-query-hbase"></a><span data-ttu-id="ce41f-166">Använd Hive för att ställa frågor till HBase</span><span class="sxs-lookup"><span data-stu-id="ce41f-166">Use Hive to query HBase</span></span>

<span data-ttu-id="ce41f-167">Du kan fråga efter data i HBase-tabeller med hjälp av Hive.</span><span class="sxs-lookup"><span data-stu-id="ce41f-167">You can query data in HBase tables by using Hive.</span></span> <span data-ttu-id="ce41f-168">I det här avsnittet skapar du en Hive-tabell som mappar till HBase-tabellen och använder den för att fråga efter data i din HBase-tabell.</span><span class="sxs-lookup"><span data-stu-id="ce41f-168">In this section, you create a Hive table that maps to the HBase table and uses it to query the data in your HBase table.</span></span>

1. <span data-ttu-id="ce41f-169">Öppna **PuTTY** och anslut till klustret.</span><span class="sxs-lookup"><span data-stu-id="ce41f-169">Open **PuTTY**, and connect to the cluster.</span></span>  <span data-ttu-id="ce41f-170">Se anvisningarna i föregående procedur.</span><span class="sxs-lookup"><span data-stu-id="ce41f-170">See the instructions in the previous procedure.</span></span>
2. <span data-ttu-id="ce41f-171">Från SSH-sessionen använder du följande kommando för att starta Beeline:</span><span class="sxs-lookup"><span data-stu-id="ce41f-171">From the SSH session, use the following command to start Beeline:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    <span data-ttu-id="ce41f-172">Mer information om Beeline finns i [Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md) (Använda Hive med Hadoop i HDInsight med Beeline).</span><span class="sxs-lookup"><span data-stu-id="ce41f-172">For more information about Beeline, see [Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md).</span></span>
       
3. <span data-ttu-id="ce41f-173">Kör följande HiveQL-skript för att skapa en Hive-tabell som mappar till HBase-tabellen.</span><span class="sxs-lookup"><span data-stu-id="ce41f-173">Run the following HiveQL script  to create a Hive table that maps to the HBase table.</span></span> <span data-ttu-id="ce41f-174">Kontrollera att du har skapat den exempeltabell som vi hänvisade till tidigare i den här självstudien med hjälp av HBase-gränssnittet innan du kör den här instruktionen.</span><span class="sxs-lookup"><span data-stu-id="ce41f-174">Make sure that you have created the sample table referenced earlier in this tutorial by using the HBase shell before you run this statement.</span></span>

    ```hiveql   
    CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
    STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
    TBLPROPERTIES ('hbase.table.name' = 'Contacts');
    ```

4. <span data-ttu-id="ce41f-175">Kör följande HiveQL-skript för att fråga data i HBase-tabellen:</span><span class="sxs-lookup"><span data-stu-id="ce41f-175">Run the following HiveQL script to query the data in the HBase table:</span></span>

    ```hiveql   
    SELECT count(rowkey) FROM hbasecontacts;
    ```

## <a name="use-hbase-rest-apis-using-curl"></a><span data-ttu-id="ce41f-176">Använd HBase REST API:er med Curl</span><span class="sxs-lookup"><span data-stu-id="ce41f-176">Use HBase REST APIs using Curl</span></span>

<span data-ttu-id="ce41f-177">REST API skyddas via [grundläggande autentisering](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="ce41f-177">The REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="ce41f-178">Du bör alltid göra begäranden genom att använda säker HTTP (HTTPS) för att säkerställa att dina autentiseringsuppgifter skickas på ett säkert sätt till servern.</span><span class="sxs-lookup"><span data-stu-id="ce41f-178">You shall always make requests by using Secure HTTP (HTTPS) to help ensure that your credentials are securely sent to the server.</span></span>

2. <span data-ttu-id="ce41f-179">Använd följande kommando för att lista de befintliga HBase-tabellerna:</span><span class="sxs-lookup"><span data-stu-id="ce41f-179">Use the following command to list the existing HBase tables:</span></span>

    ```bash
    curl -u <UserName>:<Password> \
    -G https://<ClusterName>.azurehdinsight.net/hbaserest/
    ```

3. <span data-ttu-id="ce41f-180">Använd följande kommando för att skapa en ny HBase-tabell med två kolumnserier:</span><span class="sxs-lookup"><span data-stu-id="ce41f-180">Use the following command to create a new HBase table with two-column families:</span></span>

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
    -v
    ```

    <span data-ttu-id="ce41f-181">Schemat tillhandahålls i JSon-format.</span><span class="sxs-lookup"><span data-stu-id="ce41f-181">The schema is provided in the JSon format.</span></span>
4. <span data-ttu-id="ce41f-182">Använd följande kommando för att infoga vissa data:</span><span class="sxs-lookup"><span data-stu-id="ce41f-182">Use the following command to insert some data:</span></span>

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
    -v
    ```
   
    <span data-ttu-id="ce41f-183">Du måste base64-koda de värden som anges i switchen -d.</span><span class="sxs-lookup"><span data-stu-id="ce41f-183">You must base64 encode the values specified in the -d switch.</span></span> <span data-ttu-id="ce41f-184">I exemplet:</span><span class="sxs-lookup"><span data-stu-id="ce41f-184">In the example:</span></span>
   
   * <span data-ttu-id="ce41f-185">MTAwMA ==: 1000</span><span class="sxs-lookup"><span data-stu-id="ce41f-185">MTAwMA==: 1000</span></span>
   * <span data-ttu-id="ce41f-186">UGVyc29uYWw6TmFtZQ==: Personal:Name</span><span class="sxs-lookup"><span data-stu-id="ce41f-186">UGVyc29uYWw6TmFtZQ==: Personal:Name</span></span>
   * <span data-ttu-id="ce41f-187">Sm9obiBEb2xl: John Dole</span><span class="sxs-lookup"><span data-stu-id="ce41f-187">Sm9obiBEb2xl: John Dole</span></span>
     
     <span data-ttu-id="ce41f-188">[false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) gör att du kan infoga flera (gruppbaserade) värden.</span><span class="sxs-lookup"><span data-stu-id="ce41f-188">[false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) allows you to insert multiple (batched) values.</span></span>
5. <span data-ttu-id="ce41f-189">Använd följande kommando för att få en rad:</span><span class="sxs-lookup"><span data-stu-id="ce41f-189">Use the following command to get a row:</span></span>
   
    ```bash 
    curl -u <UserName>:<Password> \
    -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
    -H "Accept: application/json" \
    -v
    ```

<span data-ttu-id="ce41f-190">Mer information om HBase Rest finns i [Referensguiden för Apache HBase](https://hbase.apache.org/book.html#_rest).</span><span class="sxs-lookup"><span data-stu-id="ce41f-190">For more information about HBase Rest, see [Apache HBase Reference Guide](https://hbase.apache.org/book.html#_rest).</span></span>

> [!NOTE]
> <span data-ttu-id="ce41f-191">Thrift stöds inte av HBase i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ce41f-191">Thrift is not supported by HBase in HDInsight.</span></span>
>
> <span data-ttu-id="ce41f-192">När du använder Curl eller annan REST-kommunikation med WebHCat, måste du autentisera begärandena genom att ange användarnamn och lösenord för HDInsight-klustrets administratör.</span><span class="sxs-lookup"><span data-stu-id="ce41f-192">When using Curl or any other REST communication with WebHCat, you must authenticate the requests by providing the user name and password for the HDInsight cluster administrator.</span></span> <span data-ttu-id="ce41f-193">Du måste också använda klustrets namn som en del av den URI (Uniform Resource Identifier) som används för att skicka begäranden till servern.</span><span class="sxs-lookup"><span data-stu-id="ce41f-193">You must also use the cluster name as part of the Uniform Resource Identifier (URI) used to send the requests to the server:</span></span>
> 
>   
>        curl -u <UserName>:<Password> \
>        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
>   
>    <span data-ttu-id="ce41f-194">Du bör få ett svar som liknar följande svar:</span><span class="sxs-lookup"><span data-stu-id="ce41f-194">You should receive a response similar to the following response:</span></span>
>   
>        {"status":"ok","version":"v1"}
   


## <a name="check-cluster-status"></a><span data-ttu-id="ce41f-195">Kontrollera klusterstatus</span><span class="sxs-lookup"><span data-stu-id="ce41f-195">Check cluster status</span></span>
<span data-ttu-id="ce41f-196">HBase i HDInsight levereras med ett webbgränssnitt för övervakning av kluster.</span><span class="sxs-lookup"><span data-stu-id="ce41f-196">HBase in HDInsight ships with a Web UI for monitoring clusters.</span></span> <span data-ttu-id="ce41f-197">Du kan använda webbgränssnittet för att begära statistik eller information om regioner.</span><span class="sxs-lookup"><span data-stu-id="ce41f-197">Using the Web UI, you can request statistics or information about regions.</span></span>

<span data-ttu-id="ce41f-198">**Så här får du åtkomst till HBase Master UI**</span><span class="sxs-lookup"><span data-stu-id="ce41f-198">**To access the HBase Master UI**</span></span>

1. <span data-ttu-id="ce41f-199">Logga in på Ambari-webbgränssnittet på https://&lt;Clustername>.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="ce41f-199">Sign into the the Ambari Web UI at https://&lt;Clustername>.azurehdinsight.net.</span></span>
2. <span data-ttu-id="ce41f-200">Klicka på **HBase** på den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="ce41f-200">Click **HBase** from the left menu.</span></span>
3. <span data-ttu-id="ce41f-201">Klicka på **Snabblänkar** överst på sidan, peka på den aktiva Zookeeper-nodlänken och klicka sedan på **HBase Master UI**.</span><span class="sxs-lookup"><span data-stu-id="ce41f-201">Click **Quick links** on the top of the page, point to the active Zookeeper node link, and then click **HBase Master UI**.</span></span>  <span data-ttu-id="ce41f-202">Gränssnittet har öppnats i en annan webbläsarflik:</span><span class="sxs-lookup"><span data-stu-id="ce41f-202">The UI is opened in another browser tab:</span></span>

  ![HDInsight HBase HMaster UI](./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

  <span data-ttu-id="ce41f-204">HBase Master UI innehåller följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="ce41f-204">The HBase Master UI contains the following sections:</span></span>

  - <span data-ttu-id="ce41f-205">regionservrar</span><span class="sxs-lookup"><span data-stu-id="ce41f-205">region servers</span></span>
  - <span data-ttu-id="ce41f-206">huvudservrar för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="ce41f-206">backup masters</span></span>
  - <span data-ttu-id="ce41f-207">tabeller</span><span class="sxs-lookup"><span data-stu-id="ce41f-207">tables</span></span>
  - <span data-ttu-id="ce41f-208">uppgifter</span><span class="sxs-lookup"><span data-stu-id="ce41f-208">tasks</span></span>
  - <span data-ttu-id="ce41f-209">attribut för programvara</span><span class="sxs-lookup"><span data-stu-id="ce41f-209">software attributes</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="ce41f-210">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="ce41f-210">Delete the cluster</span></span>
<span data-ttu-id="ce41f-211">Om du vill undvika inkonsekvenser rekommenderar vi att du inaktiverar HBase-tabellerna innan du tar bort klustret.</span><span class="sxs-lookup"><span data-stu-id="ce41f-211">To avoid inconsistencies, we recommend that you disable the HBase tables before you delete the cluster.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="ce41f-212">Felsöka</span><span class="sxs-lookup"><span data-stu-id="ce41f-212">Troubleshoot</span></span>

<span data-ttu-id="ce41f-213">Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="ce41f-213">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ce41f-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ce41f-214">Next steps</span></span>
<span data-ttu-id="ce41f-215">I den artikeln har du fått lära dig att skapa ett HBase-kluster och hur du skapar tabeller och visar data i dessa tabeller från HBase-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="ce41f-215">In this article, you learned how to create an HBase cluster and how to create tables and view the data in those tables from the HBase shell.</span></span> <span data-ttu-id="ce41f-216">Du har också fått lära dig hur man använder en Hive-fråga på data i HBase-tabeller och hur man använder HBase C# REST-API:er för att skapa en HBase-tabell och hämta data från tabellen.</span><span class="sxs-lookup"><span data-stu-id="ce41f-216">You also learned how to use a Hive query on data in HBase tables and how to use the HBase C# REST APIs to create an HBase table and retrieve data from the table.</span></span>

<span data-ttu-id="ce41f-217">Du kan läsa mer här:</span><span class="sxs-lookup"><span data-stu-id="ce41f-217">To learn more, see:</span></span>

* <span data-ttu-id="ce41f-218">[HDInsight HBase-översikt][hdinsight-hbase-overview]: HBase är en NoSQL-databas av Apachetyp med öppen källkod som bygger på Hadoop och som ger direktåtkomst och stark konsekvens för stora mängder ostrukturerade och semistrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="ce41f-218">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>

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
