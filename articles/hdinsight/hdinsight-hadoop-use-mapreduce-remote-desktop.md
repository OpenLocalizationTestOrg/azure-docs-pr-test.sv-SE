---
title: "aaaMapReduce och Fjärrskrivbord med Hadoop i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse fjärrskrivbord tooconnect tooHadoop på HDInsight och kör MapReduce-jobb."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9d3a7b34-7def-4c2e-bb6c-52682d30dee8
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: bdbbcf59ca86dd1b170471aa9e12334a04c23667
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a><span data-ttu-id="8f1ab-103">Använda MapReduce i Hadoop i HDInsight med fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="8f1ab-103">Use MapReduce in Hadoop on HDInsight with Remote Desktop</span></span>
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="8f1ab-104">I den här artikeln kommer du lär dig hur tooconnect tooa Hadoop på HDInsight-kluster med hjälp av fjärrskrivbord och kör sedan MapReduce-jobb med hello Hadoop-kommando.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-104">In this article, you will learn how tooconnect tooa Hadoop on HDInsight cluster by using Remote Desktop and then run MapReduce jobs by using hello Hadoop command.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8f1ab-105">Fjärrskrivbord är bara tillgänglig på Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-105">Remote Desktop is only available on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="8f1ab-106">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="8f1ab-107">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="8f1ab-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="8f1ab-108">För HDInsight 3.4 eller större finns [använda MapReduce med SSH](hdinsight-hadoop-use-mapreduce-ssh.md) för information om hur du ansluter toohello HDInsight-kluster och kör MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-108">For HDInsight 3.4 or greater, see [Use MapReduce with SSH](hdinsight-hadoop-use-mapreduce-ssh.md) for information on connecting toohello HDInsight cluster and running MapReduce jobs.</span></span>

## <span data-ttu-id="8f1ab-109"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="8f1ab-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="8f1ab-110">toocomplete hello stegen i den här artikeln, behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="8f1ab-110">toocomplete hello steps in this article, you will need hello following:</span></span>

* <span data-ttu-id="8f1ab-111">Ett kluster med Windows-baserade HDInsight (Hadoop på HDInsight)</span><span class="sxs-lookup"><span data-stu-id="8f1ab-111">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="8f1ab-112">En klientdator som kör Windows 10, Windows 8 eller Windows 7</span><span class="sxs-lookup"><span data-stu-id="8f1ab-112">A client computer running Windows 10, Windows 8, or Windows 7</span></span>

## <span data-ttu-id="8f1ab-113"><a id="connect"></a>Ansluta med fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="8f1ab-113"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="8f1ab-114">Aktivera Fjärrskrivbord för hello HDInsight-kluster och sedan ansluta tooit genom att följa anvisningarna hello på [ansluta tooHDInsight kluster med RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="8f1ab-114">Enable Remote Desktop for hello HDInsight cluster, then connect tooit by following hello instructions at [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="8f1ab-115"><a id="hadoop"></a>Använd hello Hadoop-kommando</span><span class="sxs-lookup"><span data-stu-id="8f1ab-115"><a id="hadoop"></a>Use hello Hadoop command</span></span>
<span data-ttu-id="8f1ab-116">När du är ansluten toohello desktop för hello HDInsight-kluster, Använd hello följande steg toorun ett MapReduce-jobb med hello Hadoop-kommando:</span><span class="sxs-lookup"><span data-stu-id="8f1ab-116">When you are connected toohello desktop for hello HDInsight cluster, use hello following steps toorun a MapReduce job by using hello Hadoop command:</span></span>

1. <span data-ttu-id="8f1ab-117">Starta från hello HDInsight desktop hello **Hadoop kommandoraden**.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-117">From hello HDInsight desktop, start hello **Hadoop Command Line**.</span></span> <span data-ttu-id="8f1ab-118">Då öppnas en ny kommandotolk i hello **c:\apps\dist\hadoop-&lt;versionsnummer >** directory.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-118">This opens a new command prompt in hello **c:\apps\dist\hadoop-&lt;version number>** directory.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8f1ab-119">hello versionsnumret ändras när Hadoop uppdateras.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-119">hello version number changes as Hadoop is updated.</span></span> <span data-ttu-id="8f1ab-120">Hej **HADOOP_HOME** miljövariabeln kan vara används toofind hello sökväg.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-120">hello **HADOOP_HOME** environment variable can be used toofind hello path.</span></span> <span data-ttu-id="8f1ab-121">Till exempel `cd %HADOOP_HOME%` ändringar kataloger toohello Hadoop katalog, utan att du tooknow hello-versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-121">For example, `cd %HADOOP_HOME%` changes directories toohello Hadoop directory, without requiring you tooknow hello version number.</span></span>
   >
   >
2. <span data-ttu-id="8f1ab-122">toouse hello **Hadoop** kommandot toorun ett exempel MapReduce-jobb, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8f1ab-122">toouse hello **Hadoop** command toorun an example MapReduce job, use hello following command:</span></span>

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    <span data-ttu-id="8f1ab-123">Detta startar hello **wordcount** -klassen, som ingår i hello **hadoop-mapreduce-examples.jar** filen i hello aktuell katalog.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-123">This starts hello **wordcount** class, which is contained in hello **hadoop-mapreduce-examples.jar** file in hello current directory.</span></span> <span data-ttu-id="8f1ab-124">Som indata, används hello **wasb://example/data/gutenberg/davinci.txt** dokument och utdata är lagrade på: **wasb: / / / exempel/data/WordCountOutput**.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-124">As input, it uses hello **wasb://example/data/gutenberg/davinci.txt** document, and output is stored at: **wasb:///example/data/WordCountOutput**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8f1ab-125">Mer information om den här MapReduce-jobb och hello exempeldata finns <a href="hdinsight-use-mapreduce.md">använda MapReduce i HDInsight Hadoop</a>.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-125">for more information about this MapReduce job and hello example data, see <a href="hdinsight-use-mapreduce.md">Use MapReduce in HDInsight Hadoop</a>.</span></span>
   >
   >
3. <span data-ttu-id="8f1ab-126">hello jobbet sänder ut information när den bearbetas och returnerar information liknande toohello följande när hello jobbet har slutförts:</span><span class="sxs-lookup"><span data-stu-id="8f1ab-126">hello job emits details as it is processed, and it returns information similar toohello following when hello job is complete:</span></span>

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. <span data-ttu-id="8f1ab-127">När hello jobbet har slutförts, kan du använda hello följande kommando toolist hello utgående filer lagras på **wasb://example/data/WordCountOutput**:</span><span class="sxs-lookup"><span data-stu-id="8f1ab-127">When hello job is complete, use hello following command toolist hello output files stored at **wasb://example/data/WordCountOutput**:</span></span>

        hadoop fs -ls wasb:///example/data/WordCountOutput

    <span data-ttu-id="8f1ab-128">Visas två filer, **_SUCCESS** och **del-r-00000**.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-128">This should display two files, **_SUCCESS** and **part-r-00000**.</span></span> <span data-ttu-id="8f1ab-129">Hej **del-r-00000** filen innehåller hello utdata för jobbet.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-129">hello **part-r-00000** file contains hello output for this job.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8f1ab-130">Vissa MapReduce-jobb kan delas upp hello resultat över flera **del-r-###** filer.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-130">Some MapReduce jobs may split hello results across multiple **part-r-#####** files.</span></span> <span data-ttu-id="8f1ab-131">I så fall använder hello ### suffix tooindicate hello ordning hello-filer.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-131">If so, use hello ##### suffix tooindicate hello order of hello files.</span></span>
   >
   >
5. <span data-ttu-id="8f1ab-132">tooview hello utdata, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8f1ab-132">tooview hello output, use hello following command:</span></span>

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    <span data-ttu-id="8f1ab-133">Detta visar en lista över hello ord som ingår i hello **wasb://example/data/gutenberg/davinci.txt** fil och hello antal varje ord inträffade.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-133">This displays a list of hello words that are contained in hello **wasb://example/data/gutenberg/davinci.txt** file, along with hello number of times each word occured.</span></span> <span data-ttu-id="8f1ab-134">hello följande är ett exempel på hello data som ska ingå i hello-filen:</span><span class="sxs-lookup"><span data-stu-id="8f1ab-134">hello following is an example of hello data that will be contained in hello file:</span></span>

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <span data-ttu-id="8f1ab-135"><a id="summary"></a>Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="8f1ab-135"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="8f1ab-136">Som du ser ger hello Hadoop-kommandot ett enkelt sätt toorun MapReduce-jobb på ett HDInsight-kluster och sedan visa hello jobbutdata.</span><span class="sxs-lookup"><span data-stu-id="8f1ab-136">As you can see, hello Hadoop command provides an easy way toorun MapReduce jobs on an HDInsight cluster and then view hello job output.</span></span>

## <span data-ttu-id="8f1ab-137"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f1ab-137"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="8f1ab-138">Allmän information om MapReduce-jobb i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="8f1ab-138">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="8f1ab-139">Använda MapReduce på Hadoop och HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f1ab-139">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="8f1ab-140">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="8f1ab-140">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="8f1ab-141">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f1ab-141">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="8f1ab-142">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f1ab-142">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
