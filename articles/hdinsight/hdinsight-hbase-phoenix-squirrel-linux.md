---
title: aaaUse Apache Phoenix och SQuirreL med HBase - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Apache Phoenix i HDInsight, och hur tooinstall och konfigurera SQuirreL på din arbetsstation tooconnect tooan HBase-kluster i HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: cda0f33b-a2e8-494c-972f-ae0bb482b818
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/26/2017
ms.author: jgao
ms.openlocfilehash: a63e8c8212b7a992453ec94fa638ec3863a0ede3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a><span data-ttu-id="4c6ae-103">Använda Apache Phoenix med Linux-baserade HBase-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="4c6ae-103">Use Apache Phoenix with Linux-based HBase clusters in HDInsight</span></span>
<span data-ttu-id="4c6ae-104">Lär dig hur toouse [Apache Phoenix](http://phoenix.apache.org/) i HDInsight, och hur toouse SQLLine.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-104">Learn how toouse [Apache Phoenix](http://phoenix.apache.org/) in HDInsight, and how toouse SQLLine.</span></span> <span data-ttu-id="4c6ae-105">Läs mer om Phoenix [Phoenix i 15 minuter eller mindre](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span><span class="sxs-lookup"><span data-stu-id="4c6ae-105">For more information about Phoenix, see [Phoenix in 15 minutes or less](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span></span> <span data-ttu-id="4c6ae-106">Hello Phoenix grammatik, se [Phoenix grammatik](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="4c6ae-106">For hello Phoenix grammar, see [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

> [!NOTE]
> <span data-ttu-id="4c6ae-107">Hello Phoenix versionsinformation i HDInsight, se [vad är nytt i hello Hadoop-klusterversioner som tillhandahålls av HDInsight?](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="4c6ae-107">For hello Phoenix version information in HDInsight, see [What's new in hello Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md).</span></span>
>
>

## <a name="use-sqlline"></a><span data-ttu-id="4c6ae-108">Använd SQLLine</span><span class="sxs-lookup"><span data-stu-id="4c6ae-108">Use SQLLine</span></span>
<span data-ttu-id="4c6ae-109">[SQLLine](http://sqlline.sourceforge.net/) är ett kommandoradsverktyg tooexecute SQL.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-109">[SQLLine](http://sqlline.sourceforge.net/) is a command-line utility tooexecute SQL.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="4c6ae-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4c6ae-110">Prerequisites</span></span>
<span data-ttu-id="4c6ae-111">Innan du kan använda SQLLine, måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="4c6ae-111">Before you can use SQLLine, you must have hello following:</span></span>

* <span data-ttu-id="4c6ae-112">**Ett HBase-kluster i HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-112">**An HBase cluster in HDInsight**.</span></span> <span data-ttu-id="4c6ae-113">Mer information om etablera HBase-kluster, se [Kom igång med Apache HBase i HDInsight][hdinsight-hbase-get-started].</span><span class="sxs-lookup"><span data-stu-id="4c6ae-113">For information on provision HBase cluster, see [Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started].</span></span>
* <span data-ttu-id="4c6ae-114">**Ansluta toohello HBase-kluster via hello Fjärrskrivbordsprotokollet**.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-114">**Connect toohello HBase cluster via hello remote desktop protocol**.</span></span> <span data-ttu-id="4c6ae-115">Instruktioner finns i [hantera Hadoop-kluster i HDInsight med hjälp av hello Azure-portalen][hdinsight-manage-portal].</span><span class="sxs-lookup"><span data-stu-id="4c6ae-115">For instructions, see [Manage Hadoop clusters in HDInsight by using hello Azure portal][hdinsight-manage-portal].</span></span>

<span data-ttu-id="4c6ae-116">När du ansluter tooan HBase-kluster måste tooconnect tooone av hello Zookeepers.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-116">When you connect tooan HBase cluster, you need tooconnect tooone of hello Zookeepers.</span></span> <span data-ttu-id="4c6ae-117">Varje HDInsight-kluster har tre Zookeepers.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-117">Each HDInsight cluster has three Zookeepers.</span></span>

<span data-ttu-id="4c6ae-118">**toofind ut hello Zookeeper-värdnamn**</span><span class="sxs-lookup"><span data-stu-id="4c6ae-118">**toofind out hello Zookeeper host name**</span></span>

1. <span data-ttu-id="4c6ae-119">Öppna Ambari genom att bläddra för**https://<ClusterName>. azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-119">Open Ambari by browsing too**https://<ClusterName>.azurehdinsight.net**.</span></span>
2. <span data-ttu-id="4c6ae-120">Ange hello HTTP (kluster) användarnamn och lösenord toologin.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-120">Enter hello HTTP (cluster) username and password toologin.</span></span>
3. <span data-ttu-id="4c6ae-121">Klicka på **ZooKeeper** hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-121">Click **ZooKeeper** from hello left menu.</span></span> <span data-ttu-id="4c6ae-122">Du ser tre **ZooKeeper-Server** visas.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-122">You see three **ZooKeeper Server** listed.</span></span>
4. <span data-ttu-id="4c6ae-123">Klicka på någon av hello **ZooKeeper-Server** visas.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-123">Click one of hello **ZooKeeper Server** listed.</span></span> <span data-ttu-id="4c6ae-124">Hitta hello på hello sammanfattningsfönstret, **värdnamn**.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-124">On hello Summary pane, find hello **Hostname**.</span></span> <span data-ttu-id="4c6ae-125">Det är liknande för*zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-125">It is similar too*zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.</span></span>

<span data-ttu-id="4c6ae-126">**toouse SQLLine**</span><span class="sxs-lookup"><span data-stu-id="4c6ae-126">**toouse SQLLine**</span></span>

1. <span data-ttu-id="4c6ae-127">Ansluta toohello kluster med SSH.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-127">Connect toohello cluster using SSH.</span></span> <span data-ttu-id="4c6ae-128">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="4c6ae-128">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="4c6ae-129">Från SSH kör du följande kommandon toorun SQLLine hello:</span><span class="sxs-lookup"><span data-stu-id="4c6ae-129">From SSH, run hello following commands toorun SQLLine:</span></span>

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure
3. <span data-ttu-id="4c6ae-130">Kör följande kommandon toocreate hello HBase-tabellen och infoga data:</span><span class="sxs-lookup"><span data-stu-id="4c6ae-130">Run hello following commands toocreate a HBase table, and insert some data:</span></span>

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

<span data-ttu-id="4c6ae-131">Mer information finns i [SQLLine manuell](http://sqlline.sourceforge.net/#manual) och [Phoenix grammatik](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="4c6ae-131">For more information, see [SQLLine manual](http://sqlline.sourceforge.net/#manual) and [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c6ae-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4c6ae-132">Next steps</span></span>
<span data-ttu-id="4c6ae-133">I den här artikeln har du lärt dig hur toouse Apache Phoenix i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-133">In this article, you have learned how toouse Apache Phoenix in HDInsight.</span></span>  <span data-ttu-id="4c6ae-134">Det finns fler toolearn:</span><span class="sxs-lookup"><span data-stu-id="4c6ae-134">toolearn more, see:</span></span>

* <span data-ttu-id="4c6ae-135">[HDInsight HBase-översikt][hdinsight-hbase-overview]: HBase är en NoSQL-databas av Apachetyp med öppen källkod som bygger på Hadoop och som ger direktåtkomst och stark konsekvens för stora mängder ostrukturerade och semistrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-135">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>
* <span data-ttu-id="4c6ae-136">[Etablera HBase-kluster i Azure Virtual Network][hdinsight-hbase-provision-vnet]: med virtuell nätverksintegration HBase-kluster kan vara distribuerade toohello samma virtuella nätverk som dina program så att program kan kommunicera med HBase direkt.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-136">[Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]: With virtual network integration, HBase clusters can be deployed toohello same virtual network as your applications so that applications can communicate with HBase directly.</span></span>
* <span data-ttu-id="4c6ae-137">[Konfigurera HBase-replikering i HDInsight](hdinsight-hbase-replication.md): Lär dig hur tooconfigure HBase-replikering mellan två Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-137">[Configure HBase replication in HDInsight](hdinsight-hbase-replication.md): Learn how tooconfigure HBase replication across two Azure datacenters.</span></span>
* <span data-ttu-id="4c6ae-138">[Analysera Twitter-åsikter med HBase i HDInsight][hbase-twitter-sentiment]: Lär dig hur toodo realtid [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) av stordata med HBase i ett Hadoop-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4c6ae-138">[Analyze Twitter sentiment with HBase in HDInsight][hbase-twitter-sentiment]: Learn how toodo real-time [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) of big data by using HBase in a Hadoop cluster in HDInsight.</span></span>

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png
