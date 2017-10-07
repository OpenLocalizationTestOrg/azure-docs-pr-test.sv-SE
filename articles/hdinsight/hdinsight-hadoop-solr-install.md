---
title: "aaaUse skriptåtgärd tooinstall Solr på Hadoop - kluster i Azure | Microsoft Docs"
description: "Lär dig hur toocustomize HDInsight-kluster med Solr med skriptåtgärder."
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
ms.openlocfilehash: 022ba56b7499390a91bfe869e5069893e56b6503
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-solr-on-windows-based-hdinsight-clusters"></a><span data-ttu-id="affd2-103">Installera och använda Solr på Windows-baserade HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="affd2-103">Install and use Solr on Windows-based HDInsight clusters</span></span>

<span data-ttu-id="affd2-104">Lär dig hur toocustomize Windows-baserade HDInsight-kluster med Solr med skriptåtgärder och hur toouse Solr toosearch data.</span><span class="sxs-lookup"><span data-stu-id="affd2-104">Learn how toocustomize Windows-based HDInsight cluster with Solr using Script Action, and how toouse Solr toosearch data.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="affd2-105">hello stegen i det här dokumentet som endast fungerar med Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="affd2-105">hello steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="affd2-106">HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="affd2-106">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="affd2-107">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="affd2-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="affd2-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="affd2-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="affd2-109">Information om hur du använder Solr med ett Linux-baserade kluster finns i [installerar och använder Solr på HDinsight Hadoop-kluster (Linux)](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="affd2-109">For information on using Solr with a Linux-based cluster, see [Install and use Solr on HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-solr-install-linux.md).</span></span>


<span data-ttu-id="affd2-110">Du kan installera Solr på någon typ av kluster (Hadoop, Storm, HBase, Spark) på Azure HDInsight med hjälp av *skriptåtgärd*.</span><span class="sxs-lookup"><span data-stu-id="affd2-110">You can install Solr on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="affd2-111">Ett exempel skriptet tooinstall Solr på ett HDInsight-kluster är tillgänglig från en skrivskyddad Azure storage blob [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="affd2-111">A sample script tooinstall Solr on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

<span data-ttu-id="affd2-112">hello exempelskript fungerar bara med HDInsight-kluster av version 3.1.</span><span class="sxs-lookup"><span data-stu-id="affd2-112">hello sample script works only with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="affd2-113">Mer information om HDInsight-kluster-versioner finns [HDInsight-kluster-versioner](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="affd2-113">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="affd2-114">hello exempelskript som används i det här avsnittet skapar ett Windows-baserade Solr kluster med en viss konfiguration.</span><span class="sxs-lookup"><span data-stu-id="affd2-114">hello sample script used in this topic creates a Windows-based Solr cluster with a specific configuration.</span></span> <span data-ttu-id="affd2-115">Om du vill tooconfigure hello Solr kluster med olika samlingar, delar, scheman, repliker, etc., måste du ändra hello skript och Solr binärfiler därefter.</span><span class="sxs-lookup"><span data-stu-id="affd2-115">If you want tooconfigure hello Solr cluster with different collections, shards, schemas, replicas, etc., you must modify hello script and Solr binaries accordingly.</span></span>

<span data-ttu-id="affd2-116">**Relaterade artiklar**</span><span class="sxs-lookup"><span data-stu-id="affd2-116">**Related articles**</span></span>

* [<span data-ttu-id="affd2-117">Installera och använda Solr på HDinsight Hadoop-kluster (Linux)</span><span class="sxs-lookup"><span data-stu-id="affd2-117">Install and use Solr on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-solr-install-linux.md)
* <span data-ttu-id="affd2-118">[Skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md): allmän information om hur du skapar HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="affd2-118">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="affd2-119">[Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="affd2-119">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="affd2-120">[Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="affd2-120">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>

## <a name="what-is-solr"></a><span data-ttu-id="affd2-121">Vad är Solr?</span><span class="sxs-lookup"><span data-stu-id="affd2-121">What is Solr?</span></span>
<span data-ttu-id="affd2-122"><a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> är en enterprise search-plattform som gör att kraftfulla fulltextsökning i data.</span><span class="sxs-lookup"><span data-stu-id="affd2-122"><a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> is an enterprise search platform that enables powerful full-text search on data.</span></span> <span data-ttu-id="affd2-123">Hadoop gör det möjligt för lagring och hantering av stora mängder data, ger Apache Solr hello sökfunktioner tooquickly hämta hello data.</span><span class="sxs-lookup"><span data-stu-id="affd2-123">While Hadoop enables storing and managing vast amounts of data, Apache Solr provides hello search capabilities tooquickly retrieve hello data.</span></span>

## <a name="install-solr-using-portal"></a><span data-ttu-id="affd2-124">Installera Solr med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="affd2-124">Install Solr using portal</span></span>
1. <span data-ttu-id="affd2-125">Skapa ett kluster med hjälp av hello **skapa anpassade** alternativ, enligt beskrivningen i [skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="affd2-125">Start creating a cluster by using hello **CUSTOM CREATE** option, as described at [Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md).</span></span>
2. <span data-ttu-id="affd2-126">På hello **skriptåtgärder** sidan hello guiden klickar du på **lägga till skriptåtgärd** tooprovide information om hello skriptåtgärd enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="affd2-126">On hello **Script Actions** page of hello wizard, click **add script action** tooprovide details about hello script action, as shown below:</span></span>

    <span data-ttu-id="affd2-127">![Använd skriptåtgärder toocustomize ett kluster](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Använd skriptåtgärder toocustomize ett kluster")</span><span class="sxs-lookup"><span data-stu-id="affd2-127">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Use Script Action toocustomize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="affd2-128">Egenskap</span><span class="sxs-lookup"><span data-stu-id="affd2-128">Property</span></span></th><th><span data-ttu-id="affd2-129">Värde</span><span class="sxs-lookup"><span data-stu-id="affd2-129">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="affd2-130">Namn</span><span class="sxs-lookup"><span data-stu-id="affd2-130">Name</span></span></td>
            <td><span data-ttu-id="affd2-131">Ange ett namn för hello skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="affd2-131">Specify a name for hello script action.</span></span> <span data-ttu-id="affd2-132">Till exempel <b>installera Solr</b>.</span><span class="sxs-lookup"><span data-stu-id="affd2-132">For example, <b>Install Solr</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="affd2-133">Skript-URI</span><span class="sxs-lookup"><span data-stu-id="affd2-133">Script URI</span></span></td>
            <td><span data-ttu-id="affd2-134">Ange hello identifierare URI (Uniform Resource) toohello skript som är anropade toocustomize hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="affd2-134">Specify hello Uniform Resource Identifier (URI) toohello script that is invoked toocustomize hello cluster.</span></span> <span data-ttu-id="affd2-135">Till exempel <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></span><span class="sxs-lookup"><span data-stu-id="affd2-135">For example, <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="affd2-136">Nodtyp</span><span class="sxs-lookup"><span data-stu-id="affd2-136">Node Type</span></span></td>
            <td><span data-ttu-id="affd2-137">Ange hello noder som hello anpassning skript körs.</span><span class="sxs-lookup"><span data-stu-id="affd2-137">Specify hello nodes on which hello customization script is run.</span></span> <span data-ttu-id="affd2-138">Du kan välja <b>alla noder</b>, <b>huvudnoder endast</b>, eller <b>arbetarnoder endast</b>.</span><span class="sxs-lookup"><span data-stu-id="affd2-138">You can choose <b>All nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes only</b>.</span></span>
        <tr><td><span data-ttu-id="affd2-139">Parametrar</span><span class="sxs-lookup"><span data-stu-id="affd2-139">Parameters</span></span></td>
            <td><span data-ttu-id="affd2-140">Ange hello parametrar, om det krävs av hello skript.</span><span class="sxs-lookup"><span data-stu-id="affd2-140">Specify hello parameters, if required by hello script.</span></span> <span data-ttu-id="affd2-141">hello skriptet tooinstall Solr kräver inte parametrar, så du kan lämna den tom.</span><span class="sxs-lookup"><span data-stu-id="affd2-141">hello script tooinstall Solr does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="affd2-142">Du kan lägga till fler än en skript åtgärd tooinstall flera komponenter på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="affd2-142">You can add more than one script action tooinstall multiple components on hello cluster.</span></span> <span data-ttu-id="affd2-143">När du har lagt till hello skript klickar du på hello markering toostart skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="affd2-143">After you have added hello scripts, click hello checkmark toostart creating hello cluster.</span></span>

## <a name="use-solr"></a><span data-ttu-id="affd2-144">Använd Solr</span><span class="sxs-lookup"><span data-stu-id="affd2-144">Use Solr</span></span>
<span data-ttu-id="affd2-145">Du måste börja med att indexera Solr med vissa datafiler.</span><span class="sxs-lookup"><span data-stu-id="affd2-145">You must start with indexing Solr with some data files.</span></span> <span data-ttu-id="affd2-146">Du kan sedan använda Solr toorun sökningar på hello indexerade data.</span><span class="sxs-lookup"><span data-stu-id="affd2-146">You can then use Solr toorun search queries on hello indexed data.</span></span> <span data-ttu-id="affd2-147">Utför följande steg toouse Solr i ett HDInsight-kluster hello:</span><span class="sxs-lookup"><span data-stu-id="affd2-147">Perform hello following steps toouse Solr in an HDInsight cluster:</span></span>

1. <span data-ttu-id="affd2-148">**Använda Remote Desktop Protocol (RDP) tooremote till hello HDInsight-kluster med installerat Solr**.</span><span class="sxs-lookup"><span data-stu-id="affd2-148">**Use Remote Desktop Protocol (RDP) tooremote into hello HDInsight cluster with Solr installed**.</span></span> <span data-ttu-id="affd2-149">Aktivera Fjärrskrivbord för hello-kluster som du skapade med Solr installerad och sedan fjärråtkomst till hello kluster från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="affd2-149">From hello Azure portal, enable Remote Desktop for hello cluster you created with Solr installed, and then remote into hello cluster.</span></span> <span data-ttu-id="affd2-150">Instruktioner finns i [ansluta tooHDInsight kluster med RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="affd2-150">For instructions, see [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>
2. <span data-ttu-id="affd2-151">**Index Solr genom att överföra filer**.</span><span class="sxs-lookup"><span data-stu-id="affd2-151">**Index Solr by uploading data files**.</span></span> <span data-ttu-id="affd2-152">När du indexera Solr placera dokument i den som du kan behöva toosearch på.</span><span class="sxs-lookup"><span data-stu-id="affd2-152">When you index Solr, you put documents in it that you may need toosearch on.</span></span> <span data-ttu-id="affd2-153">tooindex Solr, Använd RDP tooremote i hello kluster, navigera toohello skrivbordet, öppna hello Hadoop-kommandoraden och navigera för**C:\apps\dist\solr-4.7.2\example\exampledocs**.</span><span class="sxs-lookup"><span data-stu-id="affd2-153">tooindex Solr, use RDP tooremote into hello cluster, navigate toohello desktop, open hello Hadoop command line, and navigate too**C:\apps\dist\solr-4.7.2\example\exampledocs**.</span></span> <span data-ttu-id="affd2-154">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="affd2-154">Run hello following command:</span></span>

        java -jar post.jar solr.xml monitor.xml

    <span data-ttu-id="affd2-155">Hello efter hello konsolen utdata visas:</span><span class="sxs-lookup"><span data-stu-id="affd2-155">You'll see hello following output on hello console:</span></span>

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes toohttp://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    <span data-ttu-id="affd2-156">Hej post.jar verktyget indexerar Solr två exempel dokument **solr.xml** och **monitor.xml**.</span><span class="sxs-lookup"><span data-stu-id="affd2-156">hello post.jar utility indexes Solr with two sample documents, **solr.xml** and **monitor.xml**.</span></span> <span data-ttu-id="affd2-157">Hej post.jar verktyget och hello exempeldokument är tillgängliga med Solr installation.</span><span class="sxs-lookup"><span data-stu-id="affd2-157">hello post.jar utility and hello sample documents are available with Solr installation.</span></span>
3. <span data-ttu-id="affd2-158">**Använd hello Solr instrumentpanelen toosearch inom hello indexerade dokument**.</span><span class="sxs-lookup"><span data-stu-id="affd2-158">**Use hello Solr dashboard toosearch within hello indexed documents**.</span></span> <span data-ttu-id="affd2-159">I hello RDP-session toohello HDInsight-kluster, öppna Internet Explorer och starta hello Solr instrumentpanelen på **#-http://headnodehost:8983/solr/**.</span><span class="sxs-lookup"><span data-stu-id="affd2-159">In hello RDP session toohello HDInsight cluster, open Internet Explorer, and launch hello Solr dashboard at **http://headnodehost:8983/solr/#/**.</span></span> <span data-ttu-id="affd2-160">Från hello vänster från hello **Core Selector** listrutan, Välj **collection1**, och i, klickar du på **frågan**.</span><span class="sxs-lookup"><span data-stu-id="affd2-160">From hello left pane, from hello **Core Selector** drop-down, select **collection1**, and within that, click **Query**.</span></span> <span data-ttu-id="affd2-161">Som exempel, tooselect och returnera ange alla hello dokument i Solr, hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="affd2-161">As an example, tooselect and return all hello docs in Solr, provide hello following values:</span></span>

   * <span data-ttu-id="affd2-162">I hello **q** text Ange  **\*:**\*.</span><span class="sxs-lookup"><span data-stu-id="affd2-162">In hello **q** text box, enter **\*:**\*.</span></span> <span data-ttu-id="affd2-163">Detta returnerar alla hello-dokument som indexeras i Solr.</span><span class="sxs-lookup"><span data-stu-id="affd2-163">This will return all hello documents that are indexed in Solr.</span></span> <span data-ttu-id="affd2-164">Om du vill använda toosearch för en specifik sträng inom hello dokument kan du ange den här strängen.</span><span class="sxs-lookup"><span data-stu-id="affd2-164">If you want toosearch for a specific string within hello documents, you can enter that string here.</span></span>
   * <span data-ttu-id="affd2-165">I hello **wt** textruta, Välj hello utdataformat.</span><span class="sxs-lookup"><span data-stu-id="affd2-165">In hello **wt** text box, select hello output format.</span></span> <span data-ttu-id="affd2-166">Standardvärdet är **json**.</span><span class="sxs-lookup"><span data-stu-id="affd2-166">Default is **json**.</span></span> <span data-ttu-id="affd2-167">Klicka på **köra frågan**.</span><span class="sxs-lookup"><span data-stu-id="affd2-167">Click **Execute Query**.</span></span>

     <span data-ttu-id="affd2-168">![Använd skriptåtgärder toocustomize ett kluster](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "köra en fråga på Solr instrumentpanelen")</span><span class="sxs-lookup"><span data-stu-id="affd2-168">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Run a query on Solr dashboard")</span></span>

     <span data-ttu-id="affd2-169">hello utdata returnerar hello två dokument som vi använde för fulltextindexering Solr.</span><span class="sxs-lookup"><span data-stu-id="affd2-169">hello output returns hello two docs that we used for indexing Solr.</span></span> <span data-ttu-id="affd2-170">hello utdata liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="affd2-170">hello output resembles hello following:</span></span>

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, hello Enterprise Search Server",
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
                     "Scalability - Efficient Replication tooother Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over hello e)"
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
4. <span data-ttu-id="affd2-171">**Rekommenderat: Säkerhetskopiera hello indexerade data från Solr tooAzure Blob storage som är associerade med hello HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="affd2-171">**Recommended: Back up hello indexed data from Solr tooAzure Blob storage associated with hello HDInsight cluster**.</span></span> <span data-ttu-id="affd2-172">Ett bra tips är bör du säkerhetskopiera hello indexerade data från hello Solr klusternoder till Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="affd2-172">As a good practice, you should back up hello indexed data from hello Solr cluster nodes onto Azure Blob storage.</span></span> <span data-ttu-id="affd2-173">Utför följande steg toodo så hello:</span><span class="sxs-lookup"><span data-stu-id="affd2-173">Perform hello following steps toodo so:</span></span>

   1. <span data-ttu-id="affd2-174">Öppna Internet Explorer och punkt toohello följande URL: en från hello RDP-session:</span><span class="sxs-lookup"><span data-stu-id="affd2-174">From hello RDP session, open Internet Explorer, and point toohello following URL:</span></span>

           http://localhost:8983/solr/replication?command=backup

       <span data-ttu-id="affd2-175">Du bör se ett svar som detta:</span><span class="sxs-lookup"><span data-stu-id="affd2-175">You should see a response like this:</span></span>

           <?xml version="1.0" encoding="UTF-8"?>
           <response>
             <lst name="responseHeader">
               <int name="status">0</int>
               <int name="QTime">9</int>
             </lst>
             <str name="status">OK</str>
           </response>
   2. <span data-ttu-id="affd2-176">Hello fjärrsession navigera för {SOLR_HOME}\{samling} \data.</span><span class="sxs-lookup"><span data-stu-id="affd2-176">In hello remote session, navigate too{SOLR_HOME}\{Collection}\data.</span></span> <span data-ttu-id="affd2-177">Detta bör vara för hello-kluster som har skapats via hello exempelskript **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**.</span><span class="sxs-lookup"><span data-stu-id="affd2-177">For hello cluster created via hello sample script, this should be **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**.</span></span> <span data-ttu-id="affd2-178">På den här platsen bör du se en mapp för ögonblicksbilder som skapats med ett namn liknande för**ögonblicksbild.* tidsstämpel***.</span><span class="sxs-lookup"><span data-stu-id="affd2-178">At this location, you should see a snapshot folder created with a name similar too**snapshot.*timestamp***.</span></span>
   3. <span data-ttu-id="affd2-179">ZIP-mapp för ögonblicksbilder för hello och överför den tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="affd2-179">Zip hello snapshot folder and upload it tooAzure Blob storage.</span></span> <span data-ttu-id="affd2-180">Från kommandoraden för hello Hadoop, navigera toohello platsen för hello ögonblicksbildmappen med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="affd2-180">From hello Hadoop command line, navigate toohello location of hello snapshot folder by using hello following command:</span></span>

             hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

       <span data-ttu-id="affd2-181">Det här kommandot kopior hello ögonblicksbild för/exempel/data/under hello behållare i hello standard Storage-kontot som associerats med hello kluster.</span><span class="sxs-lookup"><span data-stu-id="affd2-181">This command copies hello snapshot too/example/data/ under hello container within hello default Storage account associated with hello cluster.</span></span>

## <a name="install-solr-using-aure-powershell"></a><span data-ttu-id="affd2-182">Installera Solr med Aure PowerShell</span><span class="sxs-lookup"><span data-stu-id="affd2-182">Install Solr using Aure PowerShell</span></span>
<span data-ttu-id="affd2-183">Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="affd2-183">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="affd2-184">hello exemplet visar hur tooinstall Spark med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="affd2-184">hello sample demonstrates how tooinstall Spark using Azure PowerShell.</span></span> <span data-ttu-id="affd2-185">Du behöver toocustomize hello skriptet toouse [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="affd2-185">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

## <a name="install-solr-using-net-sdk"></a><span data-ttu-id="affd2-186">Installera Solr med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="affd2-186">Install Solr using .NET SDK</span></span>
<span data-ttu-id="affd2-187">Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="affd2-187">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="affd2-188">hello exemplet visar hur tooinstall Spark med hello .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="affd2-188">hello sample demonstrates how tooinstall Spark using hello .NET SDK.</span></span> <span data-ttu-id="affd2-189">Du behöver toocustomize hello skriptet toouse [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="affd2-189">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

## <a name="see-also"></a><span data-ttu-id="affd2-190">Se även</span><span class="sxs-lookup"><span data-stu-id="affd2-190">See also</span></span>
* [<span data-ttu-id="affd2-191">Installera och använda Solr på HDinsight Hadoop-kluster (Linux)</span><span class="sxs-lookup"><span data-stu-id="affd2-191">Install and use Solr on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-solr-install-linux.md)
* <span data-ttu-id="affd2-192">[Skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md): allmän information om hur du skapar HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="affd2-192">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="affd2-193">[Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="affd2-193">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="affd2-194">[Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="affd2-194">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>
* <span data-ttu-id="affd2-195">[Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]: skriptåtgärder exempel om hur du installerar Spark.</span><span class="sxs-lookup"><span data-stu-id="affd2-195">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark.</span></span>
* <span data-ttu-id="affd2-196">[Installera R på HDInsight-kluster][hdinsight-install-r]: skriptåtgärder exempel om hur du installerar R.</span><span class="sxs-lookup"><span data-stu-id="affd2-196">[Install R on HDInsight clusters][hdinsight-install-r]: Script Action sample about installing R.</span></span>
* <span data-ttu-id="affd2-197">[Installera Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md): skriptåtgärder exempel om hur du installerar Giraph.</span><span class="sxs-lookup"><span data-stu-id="affd2-197">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md): Script Action sample about installing Giraph.</span></span>

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
