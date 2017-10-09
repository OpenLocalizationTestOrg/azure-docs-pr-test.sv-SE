---
title: aaaMapReduce och SSH-anslutning med Hadoop i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toouse SSH toorun MapReduce-jobb med Hadoop i HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 844678ba-1e1f-4fda-b9ef-34df4035d547
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 9626577687fc5cc119a39d65a9c45298f57f81c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a><span data-ttu-id="3f1f5-103">Använda MapReduce med Hadoop i HDInsight med SSH</span><span class="sxs-lookup"><span data-stu-id="3f1f5-103">Use MapReduce with Hadoop on HDInsight with SSH</span></span>

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="3f1f5-104">Lär dig hur toosubmit MapReduce-jobb från en tooHDInsight för anslutning av SSH (Secure Shell).</span><span class="sxs-lookup"><span data-stu-id="3f1f5-104">Learn how toosubmit MapReduce jobs from a Secure Shell (SSH) connection tooHDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="3f1f5-105">Om du redan är bekant med Linux-baserade Hadoop-servrar, men du är ny tooHDInsight, se [Linux-baserat HDInsight tips](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="3f1f5-105">If you are already familiar with using Linux-based Hadoop servers, but you are new tooHDInsight, see [Linux-based HDInsight tips](hdinsight-hadoop-linux-information.md).</span></span>

## <span data-ttu-id="3f1f5-106"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="3f1f5-106"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="3f1f5-107">Ett kluster på Linux-baserat HDInsight (Hadoop på HDInsight)</span><span class="sxs-lookup"><span data-stu-id="3f1f5-107">A Linux-based HDInsight (Hadoop on HDInsight) cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="3f1f5-108">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3f1f5-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3f1f5-109">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="3f1f5-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="3f1f5-110">En SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="3f1f5-110">An SSH client.</span></span> <span data-ttu-id="3f1f5-111">Mer information finns i [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="3f1f5-111">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

## <span data-ttu-id="3f1f5-112"><a id="ssh"></a>Ansluta med SSH</span><span class="sxs-lookup"><span data-stu-id="3f1f5-112"><a id="ssh"></a>Connect with SSH</span></span>

<span data-ttu-id="3f1f5-113">Ansluta toohello kluster med SSH.</span><span class="sxs-lookup"><span data-stu-id="3f1f5-113">Connect toohello cluster using SSH.</span></span> <span data-ttu-id="3f1f5-114">Till exempel följande kommando hello ansluter tooa kluster med namnet **myhdinsight**:</span><span class="sxs-lookup"><span data-stu-id="3f1f5-114">For example, hello following command connects tooa cluster named **myhdinsight**:</span></span>

```bash
ssh admin@myhdinsight-ssh.azurehdinsight.net
```

<span data-ttu-id="3f1f5-115">**Om du använder en nyckel för certifikat för SSH-autentisering**, kanske du måste toospecify hello platsen för hello privat nyckel i klientsystemet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="3f1f5-115">**If you use a certificate key for SSH authentication**, you may need toospecify hello location of hello private key on your client system, for example:</span></span>

```bash
ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net
```

<span data-ttu-id="3f1f5-116">**Om du använder ett lösenord för SSH-autentisering**, behöver du tooprovide hello lösenord när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="3f1f5-116">**If you use a password for SSH authentication**, you need tooprovide hello password when prompted.</span></span>

<span data-ttu-id="3f1f5-117">Mer information om hur du använder SSH med HDInsight finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="3f1f5-117">For more information on using SSH with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="3f1f5-118"><a id="hadoop"></a>Använda Hadoop-kommandon</span><span class="sxs-lookup"><span data-stu-id="3f1f5-118"><a id="hadoop"></a>Use Hadoop commands</span></span>

1. <span data-ttu-id="3f1f5-119">När du är ansluten toohello HDInsight-kluster, kommando Använd hello följande toostart ett MapReduce-jobb:</span><span class="sxs-lookup"><span data-stu-id="3f1f5-119">After you are connected toohello HDInsight cluster, use hello following command toostart a MapReduce job:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    <span data-ttu-id="3f1f5-120">Detta kommando startar hello `wordcount` -klassen, som ingår i hello `hadoop-mapreduce-examples.jar` fil.</span><span class="sxs-lookup"><span data-stu-id="3f1f5-120">This command starts hello `wordcount` class, which is contained in hello `hadoop-mapreduce-examples.jar` file.</span></span> <span data-ttu-id="3f1f5-121">Den använder hello `/example/data/gutenberg/davinci.txt` dokumentet som indata och utdata lagras på `/example/data/WordCountOutput`.</span><span class="sxs-lookup"><span data-stu-id="3f1f5-121">It uses hello `/example/data/gutenberg/davinci.txt` document as input, and output is stored at `/example/data/WordCountOutput`.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3f1f5-122">Mer information om den här MapReduce-jobb och hello exempeldata finns [använda MapReduce i Hadoop på HDInsight](hdinsight-use-mapreduce.md).</span><span class="sxs-lookup"><span data-stu-id="3f1f5-122">For more information about this MapReduce job and hello example data, see [Use MapReduce in Hadoop on HDInsight](hdinsight-use-mapreduce.md).</span></span>

2. <span data-ttu-id="3f1f5-123">hello jobbet sänder ut information när den bearbetar och returnerar information liknande toohello efter text när hello jobbet har slutförts:</span><span class="sxs-lookup"><span data-stu-id="3f1f5-123">hello job emits details as it processes, and it returns information similar toohello following text when hello job completes:</span></span>

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. <span data-ttu-id="3f1f5-124">När hello jobbet har slutförts, kan du använda hello följande kommando toolist hello utgående filer:</span><span class="sxs-lookup"><span data-stu-id="3f1f5-124">When hello job completes, use hello following command toolist hello output files:</span></span>

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    <span data-ttu-id="3f1f5-125">Det här kommandot visar två filer, `_SUCCESS` och `part-r-00000`.</span><span class="sxs-lookup"><span data-stu-id="3f1f5-125">This command display two files, `_SUCCESS` and `part-r-00000`.</span></span> <span data-ttu-id="3f1f5-126">Hej `part-r-00000` filen innehåller hello utdata för jobbet.</span><span class="sxs-lookup"><span data-stu-id="3f1f5-126">hello `part-r-00000` file contains hello output for this job.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3f1f5-127">Vissa MapReduce-jobb kan delas upp hello resultat över flera **del-r-###** filer.</span><span class="sxs-lookup"><span data-stu-id="3f1f5-127">Some MapReduce jobs may split hello results across multiple **part-r-#####** files.</span></span> <span data-ttu-id="3f1f5-128">I så fall använder hello ### suffix tooindicate hello ordning hello-filer.</span><span class="sxs-lookup"><span data-stu-id="3f1f5-128">If so, use hello ##### suffix tooindicate hello order of hello files.</span></span>

4. <span data-ttu-id="3f1f5-129">tooview hello utdata, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3f1f5-129">tooview hello output, use hello following command:</span></span>

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    <span data-ttu-id="3f1f5-130">Detta kommando visar en lista över hello ord som ingår i hello **wasb://example/data/gutenberg/davinci.txt** fil- och hello antal varje ord inträffade.</span><span class="sxs-lookup"><span data-stu-id="3f1f5-130">This command displays a list of hello words that are contained in hello **wasb://example/data/gutenberg/davinci.txt** file and hello number of times each word occurred.</span></span> <span data-ttu-id="3f1f5-131">hello är följande ett exempel på hello data som finns i filen hello:</span><span class="sxs-lookup"><span data-stu-id="3f1f5-131">hello following text is an example of hello data that is contained in hello file:</span></span>

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <span data-ttu-id="3f1f5-132"><a id="summary"></a>Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="3f1f5-132"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="3f1f5-133">Som du kan se ger Hadoop kommandon ett enkelt sätt toorun MapReduce-jobb i ett HDInsight-kluster och sedan visa hello jobbutdata.</span><span class="sxs-lookup"><span data-stu-id="3f1f5-133">As you can see, Hadoop commands provide an easy way toorun MapReduce jobs in an HDInsight cluster and then view hello job output.</span></span>

## <span data-ttu-id="3f1f5-134"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3f1f5-134"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="3f1f5-135">Allmän information om MapReduce-jobb i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="3f1f5-135">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="3f1f5-136">Använda MapReduce på Hadoop och HDInsight</span><span class="sxs-lookup"><span data-stu-id="3f1f5-136">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="3f1f5-137">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="3f1f5-137">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="3f1f5-138">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3f1f5-138">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="3f1f5-139">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3f1f5-139">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
