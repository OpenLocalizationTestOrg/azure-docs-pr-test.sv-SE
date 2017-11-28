---
title: aaaUse interaktiva Hive i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toouse interaktiva Hive (Hive på LLAP) i HDInsight."
keywords: 
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0957643c-4936-48a3-84a3-5dc83db4ab1a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 9e751a08091d18bc1b3d070468feef87f6828c61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-interactive-hive-in-hdinsight-preview"></a><span data-ttu-id="497e8-103">Använda interaktiva Hive i HDInsight (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="497e8-103">Use Interactive Hive in HDInsight (Preview)</span></span>
<span data-ttu-id="497e8-104">Interaktiva Hive (kallas även</span><span class="sxs-lookup"><span data-stu-id="497e8-104">Interactive Hive (A.K.A.</span></span> <span data-ttu-id="497e8-105">[Live långa och processen](https://cwiki.apache.org/confluence/display/Hive/LLAP)) är en ny HDInsight [kluster typen](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="497e8-105">[Live Long and Process](https://cwiki.apache.org/confluence/display/Hive/LLAP)) is a new HDInsight [cluster type](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>  <span data-ttu-id="497e8-106">Interaktiva Hive tillåter i cacheminnet som gör Hive-frågor mycket mer interaktiva och snabbare.</span><span class="sxs-lookup"><span data-stu-id="497e8-106">Interactive Hive allows in memory caching that makes Hive queries much more interactive and faster.</span></span> <span data-ttu-id="497e8-107">Denna nya funktion gör HDInsight en hello världens de flesta performant, flexibla och öppna stordatalösningar på hello moln med InMemory-cacheminnen (med Hive och Spark) och avancerade analyser genom djupgående integrering med R-tjänster.</span><span class="sxs-lookup"><span data-stu-id="497e8-107">This new feature makes HDInsight one of hello world’s most performant, flexible, and open Big Data solutions on hello cloud with in-memory caches (using Hive and Spark) and advanced analytics through deep integration with R Services.</span></span> 

<span data-ttu-id="497e8-108">hello interaktiva Hive klustret skiljer sig från hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="497e8-108">hello Interactive Hive cluster is different from hello Hadoop cluster.</span></span> <span data-ttu-id="497e8-109">Den bara innehåller hello Hive-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="497e8-109">It only contains hello Hive service.</span></span> 

> [!NOTE]
> <span data-ttu-id="497e8-110">MapReduce, Pig, Sqoop, Oozie och andra tjänster ska tas bort från den här typen av klustret snart.</span><span class="sxs-lookup"><span data-stu-id="497e8-110">MapReduce, Pig, Sqoop, Oozie, and other services will be removed from this cluster type soon.</span></span>
> <span data-ttu-id="497e8-111">hello Hive-tjänsten i hello interaktiva Hive kluster är endast tillgänglig via hello Ambari Hive-vyn, Beeline och Hive ODBC.</span><span class="sxs-lookup"><span data-stu-id="497e8-111">hello Hive service in hello Interactive Hive cluster is only accessible via hello Ambari Hive view, Beeline, and Hive ODBC.</span></span> <span data-ttu-id="497e8-112">Det går inte att komma åt via Hive-konsolen, Templeton, Azure CLI och Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="497e8-112">It can’t be accessed via Hive console, Templeton, Azure CLI, and Azure PowerShell.</span></span> 
> 
> 

## <a name="create-an-interactive-hive-cluster"></a><span data-ttu-id="497e8-113">Skapa en interaktiv Hive-kluster</span><span class="sxs-lookup"><span data-stu-id="497e8-113">Create an Interactive Hive cluster</span></span>
<span data-ttu-id="497e8-114">Interaktiva Hive-kluster stöds bara på Linux-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="497e8-114">Interactive Hive cluster is only supported on Linux-based clusters.</span></span> <span data-ttu-id="497e8-115">Information om hur du skapar HDInsight-kluster finns i [skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="497e8-115">For information about creating HDInsight clusters, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="execute-hive-from-interactive-hive"></a><span data-ttu-id="497e8-116">Köra Hive från interaktiva Hive</span><span class="sxs-lookup"><span data-stu-id="497e8-116">Execute Hive from Interactive Hive</span></span>
<span data-ttu-id="497e8-117">Det finns olika alternativ för hur du kan köra Hive-frågor:</span><span class="sxs-lookup"><span data-stu-id="497e8-117">There are different options how you can execute Hive queries:</span></span>

* <span data-ttu-id="497e8-118">Kör Hive med hello Ambari Hive-vyn</span><span class="sxs-lookup"><span data-stu-id="497e8-118">Run Hive using hello Ambari Hive view</span></span>
  
    <span data-ttu-id="497e8-119">Hello information om hur du använder hello Hive-vy finns i [Använd hello Hive-vy med Hadoop i HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span><span class="sxs-lookup"><span data-stu-id="497e8-119">For hello information about using hello Hive View, see [Use hello Hive View with Hadoop in HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span></span>
* <span data-ttu-id="497e8-120">Kör Hive med Beeline</span><span class="sxs-lookup"><span data-stu-id="497e8-120">Run Hive using Beeline</span></span>
  
    <span data-ttu-id="497e8-121">Hello information om hur du använder Beeline på HDInsight finns i [använda Hive med Hadoop i HDInsight med Beeline](hdinsight-hadoop-use-hive-beeline.md).</span><span class="sxs-lookup"><span data-stu-id="497e8-121">For hello information on using Beeline on HDInsight, see [Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md).</span></span>
  
    <span data-ttu-id="497e8-122">Du kan använda Beeline från hello headnode eller en tom kantnod.</span><span class="sxs-lookup"><span data-stu-id="497e8-122">You use Beeline from either hello headnode or an empty edge node.</span></span>  <span data-ttu-id="497e8-123">Du bör använda Beeline från en tom kantnod.</span><span class="sxs-lookup"><span data-stu-id="497e8-123">Using Beeline from an empty edge node is recommended.</span></span>  <span data-ttu-id="497e8-124">Information om hur du skapar ett HDInsight-kluster med en tom edgenode finns [använda tom edge-noder i HDInsight](hdinsight-apps-use-edge-node.md).</span><span class="sxs-lookup"><span data-stu-id="497e8-124">For information on creating an HDInsight cluster with an empty edgenode, see [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md).</span></span>
* <span data-ttu-id="497e8-125">Kör Hive med hjälp av Hive ODBC</span><span class="sxs-lookup"><span data-stu-id="497e8-125">Run Hive using Hive ODBC</span></span>
  
    <span data-ttu-id="497e8-126">Hello information om hur du använder Hive ODBC finns [ansluta Excel tooHadoop med hello Microsoft Hive ODBC-drivrutin](hdinsight-connect-excel-hive-odbc-driver.md).</span><span class="sxs-lookup"><span data-stu-id="497e8-126">For hello information on using Hive ODBC, see [Connect Excel tooHadoop with hello Microsoft Hive ODBC driver](hdinsight-connect-excel-hive-odbc-driver.md).</span></span>

<span data-ttu-id="497e8-127">**toofind hello JDBC-anslutningssträngen:**</span><span class="sxs-lookup"><span data-stu-id="497e8-127">**toofind hello JDBC connection string:**</span></span>

1. <span data-ttu-id="497e8-128">Logga in tooAmbari med hello följande URL: https://<ClusterName>. AzureHDInsight.net.</span><span class="sxs-lookup"><span data-stu-id="497e8-128">Sign on tooAmbari using hello following URL: https://<ClusterName>.AzureHDInsight.net.</span></span>
2. <span data-ttu-id="497e8-129">Klicka på **Hive** hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="497e8-129">Click **Hive** from hello left menu.</span></span>
3. <span data-ttu-id="497e8-130">Klicka på hello markerat ikon toocopy hello-URL:</span><span class="sxs-lookup"><span data-stu-id="497e8-130">Click hello highlighted icon toocopy hello URL:</span></span>
   
   ![HDInsight Hadoop Hive interaktiva LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a><span data-ttu-id="497e8-132">Se även</span><span class="sxs-lookup"><span data-stu-id="497e8-132">See also</span></span>
* <span data-ttu-id="497e8-133">[Skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Lär dig hur toocreate interaktiva Hive-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="497e8-133">[Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Learn how toocreate Interactive Hive clusters in HDInsight.</span></span>
* <span data-ttu-id="497e8-134">[Använda Hive med Hadoop i HDInsight med Beeline](hdinsight-hadoop-use-hive-beeline.md): Lär dig hur toouse Beeline toosubmit Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="497e8-134">[Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md): Learn how toouse Beeline toosubmit Hive queries.</span></span>
* <span data-ttu-id="497e8-135">[Ansluta Excel tooHadoop med hello Microsoft Hive ODBC-drivrutin](hdinsight-connect-excel-hive-odbc-driver.md): Lär dig hur tooconnect Excel tooHive.</span><span class="sxs-lookup"><span data-stu-id="497e8-135">[Connect Excel tooHadoop with hello Microsoft Hive ODBC driver](hdinsight-connect-excel-hive-odbc-driver.md): learn how tooconnect Excel tooHive.</span></span>

