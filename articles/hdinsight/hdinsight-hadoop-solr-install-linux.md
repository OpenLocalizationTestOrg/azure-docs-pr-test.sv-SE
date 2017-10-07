---
title: "aaaUse skriptåtgärd tooinstall Solr på Linux-baserade HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur tooinstall Solr på Linux-baserade HDInsight Hadoop-kluster med skriptåtgärder."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cc93ed5c-a358-456a-91a4-f179185c0e98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: larryfr
ms.openlocfilehash: 4c179032b95ae187f1830d8927f8796372fa8ebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="3cf9a-103">Installera och använda Solr på HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="3cf9a-103">Install and use Solr on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="3cf9a-104">Lär dig hur tooinstall Solr på Azure HDInsight med hjälp av skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-104">Learn how tooinstall Solr on Azure HDInsight by using Script Action.</span></span> <span data-ttu-id="3cf9a-105">Solr är en kraftfull sökplattform och företagsnivå sökfunktioner om data som hanteras av Hadoop.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-105">Solr is a powerful search platform and provides enterprise-level search capabilities on data managed by Hadoop.</span></span>

> [!IMPORTANT]
    > <span data-ttu-id="3cf9a-106">hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-106">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="3cf9a-107">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3cf9a-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="3cf9a-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3cf9a-109">hello exempelskript som används i det här dokumentet installerar Solr 4.9 med en viss konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-109">hello sample script used in this document installs Solr 4.9 with a specific configuration.</span></span> <span data-ttu-id="3cf9a-110">Om du vill tooconfigure hello Solr kluster med olika samlingar, delar, scheman, repliker, etc., måste du ändra hello skript och Solr binärfiler.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-110">If you want tooconfigure hello Solr cluster with different collections, shards, schemas, replicas, etc., you must modify hello script and Solr binaries.</span></span>

## <span data-ttu-id="3cf9a-111"><a name="whatis"></a>Vad är Solr</span><span class="sxs-lookup"><span data-stu-id="3cf9a-111"><a name="whatis"></a>What is Solr</span></span>

<span data-ttu-id="3cf9a-112">[Apache Solr](http://lucene.apache.org/solr/features.html) är en enterprise search-plattform som gör att kraftfulla fulltextsökning i data.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-112">[Apache Solr](http://lucene.apache.org/solr/features.html) is an enterprise search platform that enables powerful full-text search on data.</span></span> <span data-ttu-id="3cf9a-113">Hadoop gör det möjligt för lagring och hantering av stora mängder data, ger Apache Solr hello sökfunktioner tooquickly hämta hello data.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-113">While Hadoop enables storing and managing vast amounts of data, Apache Solr provides hello search capabilities tooquickly retrieve hello data.</span></span>

> [!WARNING]
> <span data-ttu-id="3cf9a-114">Komponenter som ingår i hello HDInsight-kluster stöds fullt ut av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-114">Components provided with hello HDInsight cluster are fully supported by Microsoft.</span></span>
>
> <span data-ttu-id="3cf9a-115">Anpassade komponenter, till exempel Solr, ta emot kommersiellt rimliga stöd toohelp du toofurther hello felsökning.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-115">Custom components, such as Solr, receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="3cf9a-116">Microsoft-supporten kanske inte kan tooresolve problem med anpassade komponenter.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-116">Microsoft support may not be able tooresolve problems with custom components.</span></span> <span data-ttu-id="3cf9a-117">Du kan behöva tooengage hello öppen källkod communities för hjälp.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-117">You may need tooengage hello open source communities for assistance.</span></span> <span data-ttu-id="3cf9a-118">Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="3cf9a-118">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>

## <a name="what-hello-script-does"></a><span data-ttu-id="3cf9a-119">Vilka hello skriptet gör</span><span class="sxs-lookup"><span data-stu-id="3cf9a-119">What hello script does</span></span>

<span data-ttu-id="3cf9a-120">Det här skriptet gör följande ändringar toohello HDInsight-kluster hello:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-120">This script makes hello following changes toohello HDInsight cluster:</span></span>

* <span data-ttu-id="3cf9a-121">Installerar Solr 4.9 i`/usr/hdp/current/solr`</span><span class="sxs-lookup"><span data-stu-id="3cf9a-121">Installs Solr 4.9 into `/usr/hdp/current/solr`</span></span>
* <span data-ttu-id="3cf9a-122">Skapar en användare **solrusr**, vilket är att använda toorun hello Solr service</span><span class="sxs-lookup"><span data-stu-id="3cf9a-122">Creates a user, **solrusr**, which is used toorun hello Solr service</span></span>
* <span data-ttu-id="3cf9a-123">Anger **solruser** som hello ägare`/usr/hdp/current/solr`</span><span class="sxs-lookup"><span data-stu-id="3cf9a-123">Sets **solruser** as hello owner of `/usr/hdp/current/solr`</span></span>
* <span data-ttu-id="3cf9a-124">Lägger till en [Upstart](http://upstart.ubuntu.com/) konfiguration som startar Solr automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-124">Adds an [Upstart](http://upstart.ubuntu.com/) configuration that starts Solr automatically.</span></span>

## <span data-ttu-id="3cf9a-125"><a name="install"></a>Installera Solr med hjälp av skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="3cf9a-125"><a name="install"></a>Install Solr using Script Actions</span></span>

<span data-ttu-id="3cf9a-126">Ett exempel skriptet tooinstall Solr på ett HDInsight-kluster finns på följande plats hello:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-126">A sample script tooinstall Solr on an HDInsight cluster is available at hello following location:</span></span>

    https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh

<span data-ttu-id="3cf9a-127">toocreate ett kluster med installerat Solr, Använd hello stegen i hello [skapa HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-portal.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-127">toocreate a cluster that has Solr installed, use hello steps in hello [Create HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md) document.</span></span> <span data-ttu-id="3cf9a-128">Använd följande steg tooinstall Solr hello under skapandeprocessen hello:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-128">During hello creation process, use hello following steps tooinstall Solr:</span></span>

1. <span data-ttu-id="3cf9a-129">Från hello __klustret sammanfattning__ bladet, select__Advanced settings__, sedan __skript åtgärder__.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-129">From hello __Cluster summary__ blade, select__Advanced settings__, then __Script actions__.</span></span> <span data-ttu-id="3cf9a-130">Använd följande information toopopulate hello formuläret hello:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-130">Use hello following information toopopulate hello form:</span></span>

   * <span data-ttu-id="3cf9a-131">**NAMNET**: Ange ett eget namn för hello skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-131">**NAME**: Enter a friendly name for hello script action.</span></span>
   * <span data-ttu-id="3cf9a-132">**SKRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh</span><span class="sxs-lookup"><span data-stu-id="3cf9a-132">**SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh</span></span>
   * <span data-ttu-id="3cf9a-133">**HEAD**: Markera det här alternativet</span><span class="sxs-lookup"><span data-stu-id="3cf9a-133">**HEAD**: Check this option</span></span>
   * <span data-ttu-id="3cf9a-134">**WORKER**: Markera det här alternativet</span><span class="sxs-lookup"><span data-stu-id="3cf9a-134">**WORKER**: Check this option</span></span>
   * <span data-ttu-id="3cf9a-135">**ZOOKEEPER**: Kontrollera det här alternativet tooinstall på hello Zookeeper nod</span><span class="sxs-lookup"><span data-stu-id="3cf9a-135">**ZOOKEEPER**: Check this option tooinstall on hello Zookeeper node</span></span>
   * <span data-ttu-id="3cf9a-136">**PARAMETRARNA**: lämna fältet tomt</span><span class="sxs-lookup"><span data-stu-id="3cf9a-136">**PARAMETERS**: Leave this field blank</span></span>

2. <span data-ttu-id="3cf9a-137">Längst ned hello hello **skript åtgärder** blad, Använd hello **Välj** toosave hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-137">At hello bottom of hello **Script actions** blade, use hello **Select** button toosave hello configuration.</span></span> <span data-ttu-id="3cf9a-138">Använd slutligen hello **nästa** knappen tooreturn toohello __klustret sammanfattning__</span><span class="sxs-lookup"><span data-stu-id="3cf9a-138">Finally, use hello **Next** button tooreturn toohello __Cluster summary__</span></span>

3. <span data-ttu-id="3cf9a-139">Från hello __klustret sammanfattning__ väljer __skapa__ toocreate hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-139">From hello __Cluster summary__ page, select __Create__ toocreate hello cluster.</span></span>

## <span data-ttu-id="3cf9a-140"><a name="usesolr"></a>Hur använder Solr i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3cf9a-140"><a name="usesolr"></a>How do I use Solr in HDInsight</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3cf9a-141">hello stegen i det här avsnittet visar grundläggande Solr funktioner.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-141">hello steps in this section demonstrate basic Solr functionality.</span></span> <span data-ttu-id="3cf9a-142">Mer information om hur du använder Solr finns hello [Apache Solr plats](http://lucene.apache.org/solr/).</span><span class="sxs-lookup"><span data-stu-id="3cf9a-142">For more information on using Solr, see hello [Apache Solr site](http://lucene.apache.org/solr/).</span></span>

### <a name="index-data"></a><span data-ttu-id="3cf9a-143">Indexinformationen</span><span class="sxs-lookup"><span data-stu-id="3cf9a-143">Index data</span></span>

<span data-ttu-id="3cf9a-144">Använd följande steg tooadd exempel data tooSolr hello och fråga den:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-144">Use hello following steps tooadd example data tooSolr, and then query it:</span></span>

1. <span data-ttu-id="3cf9a-145">Anslut toohello HDInsight-kluster med SSH:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-145">Connect toohello HDInsight cluster using SSH:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="3cf9a-146">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="3cf9a-146">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="3cf9a-147">Stegen senare i det här dokumentet använder en SSL-tunnel tooconnect toohello Solr webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-147">Steps later in this document use an SSL tunnel tooconnect toohello Solr web UI.</span></span> <span data-ttu-id="3cf9a-148">toouse dessa steg, måste du upprätta en SSL tunnel och sedan konfigurera din webbläsare toouse den.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-148">toouse these steps, you must establish an SSL tunnel and then configure your browser toouse it.</span></span>
     >
     > <span data-ttu-id="3cf9a-149">Mer information finns i hello [använda SSH-tunnlar med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-149">For more information, see hello [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

2. <span data-ttu-id="3cf9a-150">Använd följande kommandon toohave Solr index exempeldata hello:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-150">Use hello following commands toohave Solr index sample data:</span></span>

    ```bash
    cd /usr/hdp/current/solr/example/exampledocs
    java -jar post.jar solr.xml monitor.xml
    ```

    <span data-ttu-id="3cf9a-151">hello returneras följande utdata toohello konsolen:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-151">hello following output is returned toohello console:</span></span>

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes toohttp://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    <span data-ttu-id="3cf9a-152">Hej `post.jar` verktyget lägger till hello **solr.xml** och **monitor.xml** dokument toohello index.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-152">hello `post.jar` utility adds hello **solr.xml** and **monitor.xml** documents toohello index.</span></span>
  
3. <span data-ttu-id="3cf9a-153">Använd hello följande kommando tooquery hello Solr REST API:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-153">Use hello following command tooquery hello Solr REST API:</span></span>

    ```bash
    curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"
    ```

    <span data-ttu-id="3cf9a-154">Det här kommandot söker **collection1** för alla dokument som matchar  **\*:\***  (kodad som \*% 3A\* i hello frågesträngen).</span><span class="sxs-lookup"><span data-stu-id="3cf9a-154">This command searches **collection1** for any documents matching **\*:\*** (encoded as \*%3A\* in hello query string).</span></span> <span data-ttu-id="3cf9a-155">hello följande JSON-dokumentet är ett exempel på hello svar:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-155">hello following JSON document is an example of hello response:</span></span>

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

### <a name="using-hello-solr-dashboard"></a><span data-ttu-id="3cf9a-156">Med hjälp av hello Solr instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="3cf9a-156">Using hello Solr dashboard</span></span>

<span data-ttu-id="3cf9a-157">Hej Solr instrumentpanelen är ett webbgränssnitt som låter dig toowork med Solr via webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-157">hello Solr dashboard is a web UI that allows you toowork with Solr through your web browser.</span></span> <span data-ttu-id="3cf9a-158">Hej Solr instrumentpanelen visas inte direkt på hello Internet från ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-158">hello Solr dashboard is not exposed directly on hello Internet from your HDInsight cluster.</span></span> <span data-ttu-id="3cf9a-159">Du kan använda en SSH-tunnel tooaccess den.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-159">You can use an SSH tunnel tooaccess it.</span></span> <span data-ttu-id="3cf9a-160">Mer information om hur du använder en SSH-tunnel finns hello [använda SSH-tunnlar med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-160">For more information on using an SSH tunnel, see hello [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

<span data-ttu-id="3cf9a-161">När du har skapat en SSH-tunnel använder du följande steg toouse hello Solr instrumentpanelen hello:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-161">Once you have established an SSH tunnel, use hello following steps toouse hello Solr dashboard:</span></span>

1. <span data-ttu-id="3cf9a-162">Bestäm hello värdnamnet för hello primära headnode:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-162">Determine hello host name for hello primary headnode:</span></span>

   1. <span data-ttu-id="3cf9a-163">Använda SSH tooconnect toohello klustrets huvudnod.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-163">Use SSH tooconnect toohello cluster head node.</span></span> <span data-ttu-id="3cf9a-164">Till exempel `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-164">For example, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span></span>

       <span data-ttu-id="3cf9a-165">Mer information om hur du använder SSH finns hello [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="3cf9a-165">For more information on using SSH, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

   2. <span data-ttu-id="3cf9a-166">Använd följande kommando tooget hello fullständigt kvalificerade värdnamn hello:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-166">Use hello following command tooget hello fully qualified hostname:</span></span>

        ```bash
        hostname -f
        ```

        <span data-ttu-id="3cf9a-167">Det här kommandot returnerar ett värde liknande toohello efter värdnamn:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-167">This command returns a value similar toohello following host name:</span></span>

            hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

        <span data-ttu-id="3cf9a-168">Spara hello-värdet som returneras, eftersom den används senare.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-168">Save hello value returned, as it is used later.</span></span>

2. <span data-ttu-id="3cf9a-169">Anslut för i din webbläsare**#-http://HOSTNAME:8983/solr/**, där **VÄRDNAMN** är hello namn du bestämde i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-169">In your browser, connect too**http://HOSTNAME:8983/solr/#/**, where **HOSTNAME** is hello name you determined in hello previous steps.</span></span>

    <span data-ttu-id="3cf9a-170">hello begäran dirigeras via hello SSH-tunnel toohello Solr webbgränssnittet på ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-170">hello request is routed through hello SSH tunnel toohello Solr web UI on your cluster.</span></span> <span data-ttu-id="3cf9a-171">hello visas liknande toohello följande bild:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-171">hello page appears similar toohello following image:</span></span>

    ![Bild av Solr instrumentpanelen](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)

3. <span data-ttu-id="3cf9a-173">Använd hello från vänster hello **Core Selector** listrutan tooselect **collection1**.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-173">From hello left pane, use hello **Core Selector** drop-down tooselect **collection1**.</span></span> <span data-ttu-id="3cf9a-174">Flera poster ska dem visas under **collection1**.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-174">Several entries should them appear below **collection1**.</span></span>

4. <span data-ttu-id="3cf9a-175">Från hello posterna nedan **collection1**väljer **frågan**.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-175">From hello entries below **collection1**, select **Query**.</span></span> <span data-ttu-id="3cf9a-176">Använd följande värden toopopulate hello söksidan hello:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-176">Use hello following values toopopulate hello search page:</span></span>

   * <span data-ttu-id="3cf9a-177">I hello **q** text Ange  **\*:**\*.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-177">In hello **q** text box, enter **\*:**\*.</span></span> <span data-ttu-id="3cf9a-178">Den här frågan returnerar alla hello-dokument som indexeras i Solr.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-178">This query returns all hello documents that are indexed in Solr.</span></span> <span data-ttu-id="3cf9a-179">Om du vill använda toosearch för en specifik sträng inom hello dokument kan du ange den här strängen.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-179">If you want toosearch for a specific string within hello documents, you can enter that string here.</span></span>
   * <span data-ttu-id="3cf9a-180">I hello **wt** textruta, Välj hello utdataformat.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-180">In hello **wt** text box, select hello output format.</span></span> <span data-ttu-id="3cf9a-181">Standardvärdet är **json**.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-181">Default is **json**.</span></span>

     <span data-ttu-id="3cf9a-182">Välj slutligen hello **köra frågan** knappen längst ned hello hello Sök pate.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-182">Finally, select hello **Execute Query** button at hello bottom of hello search pate.</span></span>

     ![Använd skriptåtgärder toocustomize ett kluster](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

     <span data-ttu-id="3cf9a-184">hello utdata returnerar hello två dokument som du lagt till toohello index tidigare.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-184">hello output returns hello two documents that you added toohello index earlier.</span></span> <span data-ttu-id="3cf9a-185">hello utdata är liknande toohello följande JSON-dokumentet:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-185">hello output is similar toohello following JSON document:</span></span>

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

### <a name="starting-and-stopping-solr"></a><span data-ttu-id="3cf9a-186">Starta och stoppa Solr</span><span class="sxs-lookup"><span data-stu-id="3cf9a-186">Starting and stopping Solr</span></span>

<span data-ttu-id="3cf9a-187">Använd följande kommandon toomanually stoppa och starta Solr hello:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-187">Use hello following commands toomanually stop and start Solr:</span></span>

```bash
sudo stop solr
sudo start solr
```

## <a name="backup-indexed-data"></a><span data-ttu-id="3cf9a-188">Indexerade säkerhetskopieringsdata</span><span class="sxs-lookup"><span data-stu-id="3cf9a-188">Backup indexed data</span></span>

<span data-ttu-id="3cf9a-189">Använd hello följande steg tooback Solr lagring av data toohello standard för klustret:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-189">Use hello following steps tooback up Solr data toohello default storage for your cluster:</span></span>

1. <span data-ttu-id="3cf9a-190">Ansluta toohello kluster med SSH och Använd sedan följande kommando tooget hello värdnamn för hello huvudnod hello:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-190">Connect toohello cluster using SSH, then use hello following command tooget hello host name for hello head node:</span></span>

    ```bash
    hostname -f
    ```

2. <span data-ttu-id="3cf9a-191">Använd följande kommando toocreate en ögonblicksbild av hello indexerade data hello.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-191">Use hello following command toocreate a snapshot of hello indexed data.</span></span> <span data-ttu-id="3cf9a-192">Ersätt **VÄRDNAMN** med hello namn som returnerades från föregående hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-192">Replace **HOSTNAME** with hello name returned from hello previous command:</span></span>

    ```bash
    curl http://HOSTNAME:8983/solr/replication?command=backup
    ```

    <span data-ttu-id="3cf9a-193">hello svaret är liknande toohello följande XML:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-193">hello response is similar toohello following XML:</span></span>

        <?xml version="1.0" encoding="UTF-8"?>
        <response>
          <lst name="responseHeader">
            <int name="status">0</int>
            <int name="QTime">9</int>
          </lst>
          <str name="status">OK</str>
        </response>

3. <span data-ttu-id="3cf9a-194">Ändra kataloger för`/usr/hdp/current/solr/example/solr`.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-194">Change directories too`/usr/hdp/current/solr/example/solr`.</span></span> <span data-ttu-id="3cf9a-195">Det finns en underkatalog för varje samling.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-195">There is a subdirectory here for each collection.</span></span> <span data-ttu-id="3cf9a-196">Varje samling katalogen innehåller en `data` katalog som innehåller hello ögonblicksbild för hello samling.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-196">Each collection directory contains a `data` directory that contains hello snapshot for hello collection.</span></span>

4. <span data-ttu-id="3cf9a-197">toocreate komprimerat arkiv av hello-mapp för ögonblicksbilder, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-197">toocreate a compressed archive of hello snapshot folder, use hello following command:</span></span>

    ```bash
    tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855
    ```

    <span data-ttu-id="3cf9a-198">Ersätt hello `snapshot.20150806185338855` värden med hello hello ögonblicksbild för din samling.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-198">Replace hello `snapshot.20150806185338855` values with hello name of hello snapshot for your collection.</span></span>

    <span data-ttu-id="3cf9a-199">Det här kommandot skapas ett arkiv med namnet **snapshot.20150806185338855.tgz**, som innehåller hello innehållet i hello **snapshot.20150806185338855** directory.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-199">This command creates an archive named **snapshot.20150806185338855.tgz**, which contains hello contents of hello **snapshot.20150806185338855** directory.</span></span>

5. <span data-ttu-id="3cf9a-200">Du kan sedan lagra hello Arkiv toohello klustrets primära lagring med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3cf9a-200">You can then store hello archive toohello cluster's primary storage using hello following command:</span></span>

    ```bash
    hdfs dfs -put snapshot.20150806185338855.tgz /example/data
    ```

<span data-ttu-id="3cf9a-201">Mer information om hur du arbetar med Solr säkerhetskopiering och återställning finns [https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).</span><span class="sxs-lookup"><span data-stu-id="3cf9a-201">For more information on working with Solr backup and restores, see [https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3cf9a-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3cf9a-202">Next steps</span></span>

* <span data-ttu-id="3cf9a-203">[Installera Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="3cf9a-203">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).</span></span> <span data-ttu-id="3cf9a-204">Använd kluster anpassning tooinstall Giraph på HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-204">Use cluster customization tooinstall Giraph on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="3cf9a-205">Giraph kan du tooperform diagrammet bearbetning med hjälp av Hadoop och kan användas med Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-205">Giraph allows you tooperform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span>

* <span data-ttu-id="3cf9a-206">[Installera Hue i HDInsight-kluster](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="3cf9a-206">[Install Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span> <span data-ttu-id="3cf9a-207">Använd kluster anpassning tooinstall Hue på HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-207">Use cluster customization tooinstall Hue on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="3cf9a-208">Hue är en uppsättning webbprogram används toointeract med ett Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="3cf9a-208">Hue is a set of Web applications used toointeract with a Hadoop cluster.</span></span>

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
