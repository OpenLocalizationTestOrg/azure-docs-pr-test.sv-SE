---
title: "Använda Apache Phoenix och SQuirreL med HBase - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du använder Apache Phoenix i HDInsight och hur du installerar och konfigurerar SQuirreL på din arbetsstation för att ansluta till ett HBase-kluster i HDInsight."
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
ms.openlocfilehash: 13d17083bbe26fa9745ce4c5fef9f56859243c2e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a><span data-ttu-id="215e7-103">Använda Apache Phoenix med Linux-baserade HBase-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="215e7-103">Use Apache Phoenix with Linux-based HBase clusters in HDInsight</span></span>
<span data-ttu-id="215e7-104">Lär dig hur du använder [Apache Phoenix](http://phoenix.apache.org/) i HDInsight och hur du använder SQLLine.</span><span class="sxs-lookup"><span data-stu-id="215e7-104">Learn how to use [Apache Phoenix](http://phoenix.apache.org/) in HDInsight, and how to use SQLLine.</span></span> <span data-ttu-id="215e7-105">Läs mer om Phoenix [Phoenix i 15 minuter eller mindre](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span><span class="sxs-lookup"><span data-stu-id="215e7-105">For more information about Phoenix, see [Phoenix in 15 minutes or less](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span></span> <span data-ttu-id="215e7-106">Phoenix-grammatik finns [Phoenix grammatik](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="215e7-106">For the Phoenix grammar, see [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

> [!NOTE]
> <span data-ttu-id="215e7-107">Phoenix versionsinformation i HDInsight, se [vad är nytt i Hadoop-klusterversioner som tillhandahålls av HDInsight?](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="215e7-107">For the Phoenix version information in HDInsight, see [What's new in the Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md).</span></span>
>
>

## <a name="use-sqlline"></a><span data-ttu-id="215e7-108">Använd SQLLine</span><span class="sxs-lookup"><span data-stu-id="215e7-108">Use SQLLine</span></span>
<span data-ttu-id="215e7-109">[SQLLine](http://sqlline.sourceforge.net/) är ett kommandoradsverktyg för att köra SQL.</span><span class="sxs-lookup"><span data-stu-id="215e7-109">[SQLLine](http://sqlline.sourceforge.net/) is a command-line utility to execute SQL.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="215e7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="215e7-110">Prerequisites</span></span>
<span data-ttu-id="215e7-111">Innan du kan använda SQLLine, måste du ha följande:</span><span class="sxs-lookup"><span data-stu-id="215e7-111">Before you can use SQLLine, you must have the following:</span></span>

* <span data-ttu-id="215e7-112">**Ett HBase-kluster i HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="215e7-112">**An HBase cluster in HDInsight**.</span></span> <span data-ttu-id="215e7-113">Mer information om etablera HBase-kluster, se [Kom igång med Apache HBase i HDInsight][hdinsight-hbase-get-started].</span><span class="sxs-lookup"><span data-stu-id="215e7-113">For information on provision HBase cluster, see [Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started].</span></span>
* <span data-ttu-id="215e7-114">**Ansluta till HBase-kluster via remote desktop protocol**.</span><span class="sxs-lookup"><span data-stu-id="215e7-114">**Connect to the HBase cluster via the remote desktop protocol**.</span></span> <span data-ttu-id="215e7-115">Instruktioner finns i [hantera Hadoop-kluster i HDInsight med hjälp av Azure portal][hdinsight-manage-portal].</span><span class="sxs-lookup"><span data-stu-id="215e7-115">For instructions, see [Manage Hadoop clusters in HDInsight by using the Azure portal][hdinsight-manage-portal].</span></span>

<span data-ttu-id="215e7-116">När du ansluter till ett HBase-kluster, måste du ansluta till en Zookeepers.</span><span class="sxs-lookup"><span data-stu-id="215e7-116">When you connect to an HBase cluster, you need to connect to one of the Zookeepers.</span></span> <span data-ttu-id="215e7-117">Varje HDInsight-kluster har tre Zookeepers.</span><span class="sxs-lookup"><span data-stu-id="215e7-117">Each HDInsight cluster has three Zookeepers.</span></span>

<span data-ttu-id="215e7-118">**Ta reda på Zookeeper-värdnamn**</span><span class="sxs-lookup"><span data-stu-id="215e7-118">**To find out the Zookeeper host name**</span></span>

1. <span data-ttu-id="215e7-119">Öppna Ambari genom att bläddra till **https://<ClusterName>. azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="215e7-119">Open Ambari by browsing to **https://<ClusterName>.azurehdinsight.net**.</span></span>
2. <span data-ttu-id="215e7-120">Ange HTTP (kluster)-användarnamn och lösenord för inloggning.</span><span class="sxs-lookup"><span data-stu-id="215e7-120">Enter the HTTP (cluster) username and password to login.</span></span>
3. <span data-ttu-id="215e7-121">Klicka på **ZooKeeper** i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="215e7-121">Click **ZooKeeper** from the left menu.</span></span> <span data-ttu-id="215e7-122">Du ser tre **ZooKeeper-Server** visas.</span><span class="sxs-lookup"><span data-stu-id="215e7-122">You see three **ZooKeeper Server** listed.</span></span>
4. <span data-ttu-id="215e7-123">Klicka på någon av de **ZooKeeper-Server** visas.</span><span class="sxs-lookup"><span data-stu-id="215e7-123">Click one of the **ZooKeeper Server** listed.</span></span> <span data-ttu-id="215e7-124">I rutan Sammanfattning hitta den **värdnamn**.</span><span class="sxs-lookup"><span data-stu-id="215e7-124">On the Summary pane, find the **Hostname**.</span></span> <span data-ttu-id="215e7-125">Den liknar *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="215e7-125">It is similar to *zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.</span></span>

<span data-ttu-id="215e7-126">**Att använda SQLLine**</span><span class="sxs-lookup"><span data-stu-id="215e7-126">**To use SQLLine**</span></span>

1. <span data-ttu-id="215e7-127">Ansluta till klustret med SSH.</span><span class="sxs-lookup"><span data-stu-id="215e7-127">Connect to the cluster using SSH.</span></span> <span data-ttu-id="215e7-128">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="215e7-128">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="215e7-129">Kör följande kommandon för att köra SQLLine från SSH:</span><span class="sxs-lookup"><span data-stu-id="215e7-129">From SSH, run the following commands to run SQLLine:</span></span>

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure
3. <span data-ttu-id="215e7-130">Kör följande kommandon för att skapa en HBase-tabell och infoga data:</span><span class="sxs-lookup"><span data-stu-id="215e7-130">Run the following commands to create a HBase table, and insert some data:</span></span>

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

<span data-ttu-id="215e7-131">Mer information finns i [SQLLine manuell](http://sqlline.sourceforge.net/#manual) och [Phoenix grammatik](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="215e7-131">For more information, see [SQLLine manual](http://sqlline.sourceforge.net/#manual) and [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="215e7-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="215e7-132">Next steps</span></span>
<span data-ttu-id="215e7-133">I den här artikeln har du lärt dig hur du använder Apache Phoenix i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="215e7-133">In this article, you have learned how to use Apache Phoenix in HDInsight.</span></span>  <span data-ttu-id="215e7-134">Du kan läsa mer här:</span><span class="sxs-lookup"><span data-stu-id="215e7-134">To learn more, see:</span></span>

* <span data-ttu-id="215e7-135">[HDInsight HBase-översikt][hdinsight-hbase-overview]: HBase är en NoSQL-databas av Apachetyp med öppen källkod som bygger på Hadoop och som ger direktåtkomst och stark konsekvens för stora mängder ostrukturerade och semistrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="215e7-135">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>
* <span data-ttu-id="215e7-136">[Etablera HBase-kluster i Azure Virtual Network][hdinsight-hbase-provision-vnet]: med virtuell nätverksintegration HBase-kluster kan bara distribueras till samma virtuella nätverk som dina program så att program kan kommunicera med HBase direkt.</span><span class="sxs-lookup"><span data-stu-id="215e7-136">[Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]: With virtual network integration, HBase clusters can be deployed to the same virtual network as your applications so that applications can communicate with HBase directly.</span></span>
* <span data-ttu-id="215e7-137">[Konfigurera HBase-replikering i HDInsight](hdinsight-hbase-replication.md): Lär dig att konfigurera HBase-replikering mellan två Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="215e7-137">[Configure HBase replication in HDInsight](hdinsight-hbase-replication.md): Learn how to configure HBase replication across two Azure datacenters.</span></span>
* <span data-ttu-id="215e7-138">[Analysera Twitter-åsikter med HBase i HDInsight][hbase-twitter-sentiment]: Lär dig hur du gör realtid [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) av stordata med HBase i ett Hadoop-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="215e7-138">[Analyze Twitter sentiment with HBase in HDInsight][hbase-twitter-sentiment]: Learn how to do real-time [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) of big data by using HBase in a Hadoop cluster in HDInsight.</span></span>

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
