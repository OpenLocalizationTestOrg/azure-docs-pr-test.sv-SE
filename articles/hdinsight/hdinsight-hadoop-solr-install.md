---
title: "Installera Solr på Hadoop - kluster i Azure med hjälp av skriptåtgärder | Microsoft Docs"
description: "Lär dig hur du anpassar HDInsight-kluster med Solr med skriptåtgärder."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b1e6f338-8ac1-4b38-bbb5-2f7388b9de3b
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 6efb7ea26c3cdf7748fff4b02b5810c85cc41e1a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-use-solr-on-windows-based-hdinsight-clusters"></a><span data-ttu-id="46b6f-103">Installera och använda Solr på Windows-baserade HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="46b6f-103">Install and use Solr on Windows-based HDInsight clusters</span></span>

<span data-ttu-id="46b6f-104">Lär dig hur du anpassar Windows-baserade HDInsight-kluster med Solr med skriptåtgärder och hur du använder Solr att söka efter data.</span><span class="sxs-lookup"><span data-stu-id="46b6f-104">Learn how to customize Windows-based HDInsight cluster with Solr using Script Action, and how to use Solr to search data.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="46b6f-105">Stegen i det här dokumentet fungerar endast med Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="46b6f-105">The steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="46b6f-106">HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="46b6f-106">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="46b6f-107">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="46b6f-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="46b6f-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="46b6f-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="46b6f-109">Information om hur du använder Solr med ett Linux-baserade kluster finns i [installerar och använder Solr på HDinsight Hadoop-kluster (Linux)](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="46b6f-109">For information on using Solr with a Linux-based cluster, see [Install and use Solr on HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-solr-install-linux.md).</span></span>


<span data-ttu-id="46b6f-110">Du kan installera Solr på någon typ av kluster (Hadoop, Storm, HBase, Spark) på Azure HDInsight med hjälp av *skriptåtgärd*.</span><span class="sxs-lookup"><span data-stu-id="46b6f-110">You can install Solr on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="46b6f-111">Ett exempelskript för att installera Solr på ett HDInsight-kluster är tillgänglig från en skrivskyddad Azure storage blob [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="46b6f-111">A sample script to install Solr on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

<span data-ttu-id="46b6f-112">Exempelskriptet fungerar bara med HDInsight-kluster av version 3.1.</span><span class="sxs-lookup"><span data-stu-id="46b6f-112">The sample script works only with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="46b6f-113">Mer information om HDInsight-kluster-versioner finns [HDInsight-kluster-versioner](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="46b6f-113">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="46b6f-114">Exempelskript som används i det här avsnittet skapar ett Windows-baserade Solr kluster med en viss konfiguration.</span><span class="sxs-lookup"><span data-stu-id="46b6f-114">The sample script used in this topic creates a Windows-based Solr cluster with a specific configuration.</span></span> <span data-ttu-id="46b6f-115">Om du vill konfigurera Solr klustret med olika samlingar, delar, scheman, repliker, etc. Ändra du skript och Solr binärfiler därefter.</span><span class="sxs-lookup"><span data-stu-id="46b6f-115">If you want to configure the Solr cluster with different collections, shards, schemas, replicas, etc., you must modify the script and Solr binaries accordingly.</span></span>

<span data-ttu-id="46b6f-116">**Relaterade artiklar**</span><span class="sxs-lookup"><span data-stu-id="46b6f-116">**Related articles**</span></span>

* [<span data-ttu-id="46b6f-117">Installera och använda Solr på HDinsight Hadoop-kluster (Linux)</span><span class="sxs-lookup"><span data-stu-id="46b6f-117">Install and use Solr on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-solr-install-linux.md)
* <span data-ttu-id="46b6f-118">[Skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md): allmän information om hur du skapar HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="46b6f-118">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="46b6f-119">[Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="46b6f-119">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="46b6f-120">[Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="46b6f-120">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>

## <a name="what-is-solr"></a><span data-ttu-id="46b6f-121">Vad är Solr?</span><span class="sxs-lookup"><span data-stu-id="46b6f-121">What is Solr?</span></span>
<span data-ttu-id="46b6f-122"><a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> är en enterprise search-plattform som gör att kraftfulla fulltextsökning i data.</span><span class="sxs-lookup"><span data-stu-id="46b6f-122"><a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> is an enterprise search platform that enables powerful full-text search on data.</span></span> <span data-ttu-id="46b6f-123">Hadoop gör det möjligt för lagring och hantering av stora mängder data, ger Apache Solr sökfunktioner snabbt hämta data.</span><span class="sxs-lookup"><span data-stu-id="46b6f-123">While Hadoop enables storing and managing vast amounts of data, Apache Solr provides the search capabilities to quickly retrieve the data.</span></span>

## <a name="install-solr-using-portal"></a><span data-ttu-id="46b6f-124">Installera Solr med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="46b6f-124">Install Solr using portal</span></span>
1. <span data-ttu-id="46b6f-125">Skapa ett kluster med hjälp av den **skapa anpassade** alternativ, enligt beskrivningen i [skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="46b6f-125">Start creating a cluster by using the **CUSTOM CREATE** option, as described at [Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md).</span></span>
2. <span data-ttu-id="46b6f-126">På den **skriptåtgärder** sidan i guiden klickar du på **lägga till skriptåtgärd** att ge information om skriptåtgärd, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="46b6f-126">On the **Script Actions** page of the wizard, click **add script action** to provide details about the script action, as shown below:</span></span>

    <span data-ttu-id="46b6f-127">![Använd skriptåtgärder för att anpassa ett kluster](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Använd skriptåtgärder att anpassa ett kluster")</span><span class="sxs-lookup"><span data-stu-id="46b6f-127">![Use Script Action to customize a cluster](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Use Script Action to customize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="46b6f-128">Egenskap</span><span class="sxs-lookup"><span data-stu-id="46b6f-128">Property</span></span></th><th><span data-ttu-id="46b6f-129">Värde</span><span class="sxs-lookup"><span data-stu-id="46b6f-129">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="46b6f-130">Namn</span><span class="sxs-lookup"><span data-stu-id="46b6f-130">Name</span></span></td>
            <td><span data-ttu-id="46b6f-131">Ange ett namn för skriptåtgärden.</span><span class="sxs-lookup"><span data-stu-id="46b6f-131">Specify a name for the script action.</span></span> <span data-ttu-id="46b6f-132">Till exempel <b>installera Solr</b>.</span><span class="sxs-lookup"><span data-stu-id="46b6f-132">For example, <b>Install Solr</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="46b6f-133">Skript-URI</span><span class="sxs-lookup"><span data-stu-id="46b6f-133">Script URI</span></span></td>
            <td><span data-ttu-id="46b6f-134">Ange den identifierare URI (Uniform Resource) till det skript som anropas för att anpassa klustret.</span><span class="sxs-lookup"><span data-stu-id="46b6f-134">Specify the Uniform Resource Identifier (URI) to the script that is invoked to customize the cluster.</span></span> <span data-ttu-id="46b6f-135">Till exempel <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></span><span class="sxs-lookup"><span data-stu-id="46b6f-135">For example, <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="46b6f-136">Nodtyp</span><span class="sxs-lookup"><span data-stu-id="46b6f-136">Node Type</span></span></td>
            <td><span data-ttu-id="46b6f-137">Ange de noder som anpassning skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="46b6f-137">Specify the nodes on which the customization script is run.</span></span> <span data-ttu-id="46b6f-138">Du kan välja <b>alla noder</b>, <b>huvudnoder endast</b>, eller <b>arbetarnoder endast</b>.</span><span class="sxs-lookup"><span data-stu-id="46b6f-138">You can choose <b>All nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes only</b>.</span></span>
        <tr><td><span data-ttu-id="46b6f-139">Parametrar</span><span class="sxs-lookup"><span data-stu-id="46b6f-139">Parameters</span></span></td>
            <td><span data-ttu-id="46b6f-140">Ange parametrar, om det krävs av skriptet.</span><span class="sxs-lookup"><span data-stu-id="46b6f-140">Specify the parameters, if required by the script.</span></span> <span data-ttu-id="46b6f-141">Skript för att installera Solr kräver inte parametrar, så du kan lämna den tom.</span><span class="sxs-lookup"><span data-stu-id="46b6f-141">The script to install Solr does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="46b6f-142">Du kan lägga till fler än en skriptåtgärd för att installera flera komponenter i klustret.</span><span class="sxs-lookup"><span data-stu-id="46b6f-142">You can add more than one script action to install multiple components on the cluster.</span></span> <span data-ttu-id="46b6f-143">När du har lagt till skripten, klicka på bockmarkeringen om du vill skapa klustret.</span><span class="sxs-lookup"><span data-stu-id="46b6f-143">After you have added the scripts, click the checkmark to start creating the cluster.</span></span>

## <a name="use-solr"></a><span data-ttu-id="46b6f-144">Använd Solr</span><span class="sxs-lookup"><span data-stu-id="46b6f-144">Use Solr</span></span>
<span data-ttu-id="46b6f-145">Du måste börja med att indexera Solr med vissa datafiler.</span><span class="sxs-lookup"><span data-stu-id="46b6f-145">You must start with indexing Solr with some data files.</span></span> <span data-ttu-id="46b6f-146">Du kan sedan använda Solr för att köra sökfrågor på indexerade data.</span><span class="sxs-lookup"><span data-stu-id="46b6f-146">You can then use Solr to run search queries on the indexed data.</span></span> <span data-ttu-id="46b6f-147">Utför följande steg för att använda Solr i ett HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="46b6f-147">Perform the following steps to use Solr in an HDInsight cluster:</span></span>

1. <span data-ttu-id="46b6f-148">**Använda Remote Desktop Protocol (RDP) till fjärr till HDInsight-kluster med installerat Solr**.</span><span class="sxs-lookup"><span data-stu-id="46b6f-148">**Use Remote Desktop Protocol (RDP) to remote into the HDInsight cluster with Solr installed**.</span></span> <span data-ttu-id="46b6f-149">Aktivera Fjärrskrivbord för det kluster som du skapat med Solr installerad och sedan fjärråtkomst till klustret från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="46b6f-149">From the Azure portal, enable Remote Desktop for the cluster you created with Solr installed, and then remote into the cluster.</span></span> <span data-ttu-id="46b6f-150">Instruktioner finns i [Anslut till HDInsight-kluster med RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="46b6f-150">For instructions, see [Connect to HDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>
2. <span data-ttu-id="46b6f-151">**Index Solr genom att överföra filer**.</span><span class="sxs-lookup"><span data-stu-id="46b6f-151">**Index Solr by uploading data files**.</span></span> <span data-ttu-id="46b6f-152">När du indexera Solr placera dokument i den som du kan behöva söka efter.</span><span class="sxs-lookup"><span data-stu-id="46b6f-152">When you index Solr, you put documents in it that you may need to search on.</span></span> <span data-ttu-id="46b6f-153">Indexera Solr, använda RDP att fjärråtkomst till klustret, navigera till skrivbordet, öppna kommandoraden Hadoop och navigera till **C:\apps\dist\solr-4.7.2\example\exampledocs**.</span><span class="sxs-lookup"><span data-stu-id="46b6f-153">To index Solr, use RDP to remote into the cluster, navigate to the desktop, open the Hadoop command line, and navigate to **C:\apps\dist\solr-4.7.2\example\exampledocs**.</span></span> <span data-ttu-id="46b6f-154">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="46b6f-154">Run the following command:</span></span>

        java -jar post.jar solr.xml monitor.xml

    <span data-ttu-id="46b6f-155">Följande utdata visas på konsolen:</span><span class="sxs-lookup"><span data-stu-id="46b6f-155">You'll see the following output on the console:</span></span>

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    <span data-ttu-id="46b6f-156">Verktyget post.jar indexerar Solr två exempel dokument **solr.xml** och **monitor.xml**.</span><span class="sxs-lookup"><span data-stu-id="46b6f-156">The post.jar utility indexes Solr with two sample documents, **solr.xml** and **monitor.xml**.</span></span> <span data-ttu-id="46b6f-157">Verktyget post.jar och exempeldokument är tillgängliga med Solr installation.</span><span class="sxs-lookup"><span data-stu-id="46b6f-157">The post.jar utility and the sample documents are available with Solr installation.</span></span>
3. <span data-ttu-id="46b6f-158">**Använd Solr instrumentpanelen för att söka i indexerade dokument**.</span><span class="sxs-lookup"><span data-stu-id="46b6f-158">**Use the Solr dashboard to search within the indexed documents**.</span></span> <span data-ttu-id="46b6f-159">Öppna Internet Explorer i RDP-session till HDInsight-klustret och starta Solr instrumentpanelen på **#-http://headnodehost:8983/solr/**.</span><span class="sxs-lookup"><span data-stu-id="46b6f-159">In the RDP session to the HDInsight cluster, open Internet Explorer, and launch the Solr dashboard at **http://headnodehost:8983/solr/#/**.</span></span> <span data-ttu-id="46b6f-160">I den vänstra rutan, från den **Core Selector** listrutan, Välj **collection1**, och i, klickar du på **frågan**.</span><span class="sxs-lookup"><span data-stu-id="46b6f-160">From the left pane, from the **Core Selector** drop-down, select **collection1**, and within that, click **Query**.</span></span> <span data-ttu-id="46b6f-161">Ange följande värden för att välja och återställa alla dokument i Solr, t.ex.:</span><span class="sxs-lookup"><span data-stu-id="46b6f-161">As an example, to select and return all the docs in Solr, provide the following values:</span></span>

   * <span data-ttu-id="46b6f-162">I den **q** text Ange  **\*:**\*.</span><span class="sxs-lookup"><span data-stu-id="46b6f-162">In the **q** text box, enter **\*:**\*.</span></span> <span data-ttu-id="46b6f-163">Alla dokument som indexeras returneras i Solr.</span><span class="sxs-lookup"><span data-stu-id="46b6f-163">This will return all the documents that are indexed in Solr.</span></span> <span data-ttu-id="46b6f-164">Om du vill söka efter en specifik sträng inom dokument som kan du ange den här strängen.</span><span class="sxs-lookup"><span data-stu-id="46b6f-164">If you want to search for a specific string within the documents, you can enter that string here.</span></span>
   * <span data-ttu-id="46b6f-165">I den **wt** text väljer utdataformatet.</span><span class="sxs-lookup"><span data-stu-id="46b6f-165">In the **wt** text box, select the output format.</span></span> <span data-ttu-id="46b6f-166">Standardvärdet är **json**.</span><span class="sxs-lookup"><span data-stu-id="46b6f-166">Default is **json**.</span></span> <span data-ttu-id="46b6f-167">Klicka på **köra frågan**.</span><span class="sxs-lookup"><span data-stu-id="46b6f-167">Click **Execute Query**.</span></span>

     <span data-ttu-id="46b6f-168">![Använd skriptåtgärder för att anpassa ett kluster](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "köra en fråga på Solr instrumentpanelen")</span><span class="sxs-lookup"><span data-stu-id="46b6f-168">![Use Script Action to customize a cluster](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Run a query on Solr dashboard")</span></span>

     <span data-ttu-id="46b6f-169">Utdata returnerar två dokumenten som vi använde för indexering Solr.</span><span class="sxs-lookup"><span data-stu-id="46b6f-169">The output returns the two docs that we used for indexing Solr.</span></span> <span data-ttu-id="46b6f-170">Utdata liknar följande:</span><span class="sxs-lookup"><span data-stu-id="46b6f-170">The output resembles the following:</span></span>

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, the Enterprise Search Server",
                   "manu": "Apache Software Foundation",
                   "cat": [
                     "software",
                     "search"
                   ],
                   "features": [
                     "Advanced Full-Text Search Capabilities using Lucene",
                     "Optimized for High Volume Web Traffic",
                     "Standards Based Open Interfaces - XML and HTTP",
                     "Comprehensive HTML Administration Interfaces",
                     "Scalability - Efficient Replication to other Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over the e)"
                   ],
                   "price": 0,
                   "price_c": "0,USD",
                   "popularity": 10,
                   "inStock": true,
                   "incubationdate_dt": "2006-01-17T00:00:00Z",
                   "_version_": 1486960636996878300
                 },
                 {
                   "id": "3007WFP",
                   "name": "Dell Widescreen UltraSharp 3007WFP",
                   "manu": "Dell, Inc.",
                   "manu_id_s": "dell",
                   "cat": [
                     "electronics and computer1"
                   ],
                   "features": [
                     "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                   ],
                   "includes": "USB cable",
                   "weight": 401.6,
                   "price": 2199,
                   "price_c": "2199,USD",
                   "popularity": 6,
                   "inStock": true,
                   "store": "43.17614,-90.57341",
                   "_version_": 1486960637584081000
                 }
               ]
             }
4. <span data-ttu-id="46b6f-171">**Rekommenderat: Säkerhetskopiera indexerade data från Solr till Azure Blob-lagring som associerats med HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="46b6f-171">**Recommended: Back up the indexed data from Solr to Azure Blob storage associated with the HDInsight cluster**.</span></span> <span data-ttu-id="46b6f-172">Ett bra tips är bör du säkerhetskopiera indexerade data från Solr klusternoder till Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="46b6f-172">As a good practice, you should back up the indexed data from the Solr cluster nodes onto Azure Blob storage.</span></span> <span data-ttu-id="46b6f-173">Utför följande steg för att göra det:</span><span class="sxs-lookup"><span data-stu-id="46b6f-173">Perform the following steps to do so:</span></span>

   1. <span data-ttu-id="46b6f-174">Öppna Internet Explorer RDP-session och peka på följande URL:</span><span class="sxs-lookup"><span data-stu-id="46b6f-174">From the RDP session, open Internet Explorer, and point to the following URL:</span></span>

           http://localhost:8983/solr/replication?command=backup

       <span data-ttu-id="46b6f-175">Du bör se ett svar som detta:</span><span class="sxs-lookup"><span data-stu-id="46b6f-175">You should see a response like this:</span></span>

           <?xml version="1.0" encoding="UTF-8"?>
           <response>
             <lst name="responseHeader">
               <int name="status">0</int>
               <int name="QTime">9</int>
             </lst>
             <str name="status">OK</str>
           </response>
   2. <span data-ttu-id="46b6f-176">I fjärrsessionen, navigerar du till {SOLR_HOME}\{samling} \data.</span><span class="sxs-lookup"><span data-stu-id="46b6f-176">In the remote session, navigate to {SOLR_HOME}\{Collection}\data.</span></span> <span data-ttu-id="46b6f-177">För det kluster som skapats via exempelskriptet detta bör vara **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**.</span><span class="sxs-lookup"><span data-stu-id="46b6f-177">For the cluster created via the sample script, this should be **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**.</span></span> <span data-ttu-id="46b6f-178">På den här platsen bör du se en mapp för ögonblicksbilder som skapats med ett namn som liknar  **ögonblicksbild.* tidsstämpel***.</span><span class="sxs-lookup"><span data-stu-id="46b6f-178">At this location, you should see a snapshot folder created with a name similar to **snapshot.*timestamp***.</span></span>
   3. <span data-ttu-id="46b6f-179">ZIP-mappen för ögonblicksbilder och överföra den till Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="46b6f-179">Zip the snapshot folder and upload it to Azure Blob storage.</span></span> <span data-ttu-id="46b6f-180">Navigera till platsen för mappen för ögonblicksbilder med hjälp av följande kommando från kommandoraden Hadoop:</span><span class="sxs-lookup"><span data-stu-id="46b6f-180">From the Hadoop command line, navigate to the location of the snapshot folder by using the following command:</span></span>

             hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

       <span data-ttu-id="46b6f-181">Det här kommandot kopieras ögonblicksbilden till /example/data/under behållare i standard Storage-konto som är associerade med klustret.</span><span class="sxs-lookup"><span data-stu-id="46b6f-181">This command copies the snapshot to /example/data/ under the container within the default Storage account associated with the cluster.</span></span>

## <a name="install-solr-using-aure-powershell"></a><span data-ttu-id="46b6f-182">Installera Solr med Aure PowerShell</span><span class="sxs-lookup"><span data-stu-id="46b6f-182">Install Solr using Aure PowerShell</span></span>
<span data-ttu-id="46b6f-183">Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="46b6f-183">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="46b6f-184">Exemplet visar hur du installerar Spark med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="46b6f-184">The sample demonstrates how to install Spark using Azure PowerShell.</span></span> <span data-ttu-id="46b6f-185">Du måste anpassa skript om du vill använda [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="46b6f-185">You need to customize the script to use [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

## <a name="install-solr-using-net-sdk"></a><span data-ttu-id="46b6f-186">Installera Solr med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="46b6f-186">Install Solr using .NET SDK</span></span>
<span data-ttu-id="46b6f-187">Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="46b6f-187">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="46b6f-188">Exemplet visar hur du installerar Spark med .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="46b6f-188">The sample demonstrates how to install Spark using the .NET SDK.</span></span> <span data-ttu-id="46b6f-189">Du måste anpassa skript om du vill använda [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="46b6f-189">You need to customize the script to use [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

## <a name="see-also"></a><span data-ttu-id="46b6f-190">Se även</span><span class="sxs-lookup"><span data-stu-id="46b6f-190">See also</span></span>
* [<span data-ttu-id="46b6f-191">Installera och använda Solr på HDinsight Hadoop-kluster (Linux)</span><span class="sxs-lookup"><span data-stu-id="46b6f-191">Install and use Solr on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-solr-install-linux.md)
* <span data-ttu-id="46b6f-192">[Skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md): allmän information om hur du skapar HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="46b6f-192">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="46b6f-193">[Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="46b6f-193">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="46b6f-194">[Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="46b6f-194">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>
* <span data-ttu-id="46b6f-195">[Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]: skriptåtgärder exempel om hur du installerar Spark.</span><span class="sxs-lookup"><span data-stu-id="46b6f-195">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark.</span></span>
* <span data-ttu-id="46b6f-196">[Installera R på HDInsight-kluster][hdinsight-install-r]: skriptåtgärder exempel om hur du installerar R.</span><span class="sxs-lookup"><span data-stu-id="46b6f-196">[Install R on HDInsight clusters][hdinsight-install-r]: Script Action sample about installing R.</span></span>
* <span data-ttu-id="46b6f-197">[Installera Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md): skriptåtgärder exempel om hur du installerar Giraph.</span><span class="sxs-lookup"><span data-stu-id="46b6f-197">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md): Script Action sample about installing Giraph.</span></span>

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
