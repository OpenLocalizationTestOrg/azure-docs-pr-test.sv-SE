---
title: "MapReduce och Fjärrskrivbord med Hadoop i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur du använder Fjärrskrivbord för att ansluta till Hadoop i HDInsight och kör MapReduce-jobb."
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
ms.openlocfilehash: b56674857b013f9bb3d4dd4b6e97b34e0a97b1b2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a><span data-ttu-id="66e4e-103">Använda MapReduce i Hadoop i HDInsight med fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="66e4e-103">Use MapReduce in Hadoop on HDInsight with Remote Desktop</span></span>
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="66e4e-104">I den här artikeln får lära du dig att ansluta till en Hadoop på HDInsight-kluster med hjälp av fjärrskrivbord och kör sedan MapReduce-jobb med hjälp av kommandot Hadoop.</span><span class="sxs-lookup"><span data-stu-id="66e4e-104">In this article, you will learn how to connect to a Hadoop on HDInsight cluster by using Remote Desktop and then run MapReduce jobs by using the Hadoop command.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="66e4e-105">Fjärrskrivbord är bara tillgänglig på Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="66e4e-105">Remote Desktop is only available on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="66e4e-106">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="66e4e-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="66e4e-107">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="66e4e-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="66e4e-108">För HDInsight 3.4 eller större finns [använda MapReduce med SSH](hdinsight-hadoop-use-mapreduce-ssh.md) för information om hur du ansluter till HDInsight-kluster och kör MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="66e4e-108">For HDInsight 3.4 or greater, see [Use MapReduce with SSH](hdinsight-hadoop-use-mapreduce-ssh.md) for information on connecting to the HDInsight cluster and running MapReduce jobs.</span></span>

## <span data-ttu-id="66e4e-109"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="66e4e-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="66e4e-110">Du behöver följande för att slutföra stegen i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="66e4e-110">To complete the steps in this article, you will need the following:</span></span>

* <span data-ttu-id="66e4e-111">Ett kluster med Windows-baserade HDInsight (Hadoop på HDInsight)</span><span class="sxs-lookup"><span data-stu-id="66e4e-111">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="66e4e-112">En klientdator som kör Windows 10, Windows 8 eller Windows 7</span><span class="sxs-lookup"><span data-stu-id="66e4e-112">A client computer running Windows 10, Windows 8, or Windows 7</span></span>

## <span data-ttu-id="66e4e-113"><a id="connect"></a>Ansluta med fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="66e4e-113"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="66e4e-114">Aktivera Fjärrskrivbord för HDInsight-klustret och sedan ansluta till den genom att följa anvisningarna i [Anslut till HDInsight-kluster med RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="66e4e-114">Enable Remote Desktop for the HDInsight cluster, then connect to it by following the instructions at [Connect to HDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="66e4e-115"><a id="hadoop"></a>Använd kommandot Hadoop</span><span class="sxs-lookup"><span data-stu-id="66e4e-115"><a id="hadoop"></a>Use the Hadoop command</span></span>
<span data-ttu-id="66e4e-116">När du är ansluten till skrivbordet för HDInsight-kluster kan du använda följande steg för att köra ett MapReduce-jobb med hjälp av Hadoop-kommando:</span><span class="sxs-lookup"><span data-stu-id="66e4e-116">When you are connected to the desktop for the HDInsight cluster, use the following steps to run a MapReduce job by using the Hadoop command:</span></span>

1. <span data-ttu-id="66e4e-117">HDInsight-skrivbordet och starta den **Hadoop kommandoraden**.</span><span class="sxs-lookup"><span data-stu-id="66e4e-117">From the HDInsight desktop, start the **Hadoop Command Line**.</span></span> <span data-ttu-id="66e4e-118">Då öppnas en ny kommandotolk i den **c:\apps\dist\hadoop-&lt;versionsnummer >** directory.</span><span class="sxs-lookup"><span data-stu-id="66e4e-118">This opens a new command prompt in the **c:\apps\dist\hadoop-&lt;version number>** directory.</span></span>

   > [!NOTE]
   > <span data-ttu-id="66e4e-119">Versionsnumret ändras när Hadoop uppdateras.</span><span class="sxs-lookup"><span data-stu-id="66e4e-119">The version number changes as Hadoop is updated.</span></span> <span data-ttu-id="66e4e-120">Den **HADOOP_HOME** miljövariabeln kan användas för att hitta sökvägen.</span><span class="sxs-lookup"><span data-stu-id="66e4e-120">The **HADOOP_HOME** environment variable can be used to find the path.</span></span> <span data-ttu-id="66e4e-121">Till exempel `cd %HADOOP_HOME%` ändrar kataloger till Hadoop-katalogen utan att du behöver veta versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="66e4e-121">For example, `cd %HADOOP_HOME%` changes directories to the Hadoop directory, without requiring you to know the version number.</span></span>
   >
   >
2. <span data-ttu-id="66e4e-122">Att använda den **Hadoop** kommando för att köra ett exempel MapReduce-jobb, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="66e4e-122">To use the **Hadoop** command to run an example MapReduce job, use the following command:</span></span>

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    <span data-ttu-id="66e4e-123">Detta startar den **wordcount** -klassen, som ingår i den **hadoop-mapreduce-examples.jar** fil i den aktuella katalogen.</span><span class="sxs-lookup"><span data-stu-id="66e4e-123">This starts the **wordcount** class, which is contained in the **hadoop-mapreduce-examples.jar** file in the current directory.</span></span> <span data-ttu-id="66e4e-124">Som indata, används den **wasb://example/data/gutenberg/davinci.txt** dokument och utdata är lagrade på: **wasb: / / / exempel/data/WordCountOutput**.</span><span class="sxs-lookup"><span data-stu-id="66e4e-124">As input, it uses the **wasb://example/data/gutenberg/davinci.txt** document, and output is stored at: **wasb:///example/data/WordCountOutput**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="66e4e-125">Läs mer om MapReduce-jobb och exempeldata <a href="hdinsight-use-mapreduce.md">använda MapReduce i HDInsight Hadoop</a>.</span><span class="sxs-lookup"><span data-stu-id="66e4e-125">for more information about this MapReduce job and the example data, see <a href="hdinsight-use-mapreduce.md">Use MapReduce in HDInsight Hadoop</a>.</span></span>
   >
   >
3. <span data-ttu-id="66e4e-126">Jobbet sänder ut information när den bearbetas och returnerar informationen liknar när jobbet har slutförts:</span><span class="sxs-lookup"><span data-stu-id="66e4e-126">The job emits details as it is processed, and it returns information similar to the following when the job is complete:</span></span>

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. <span data-ttu-id="66e4e-127">När jobbet är klart, använder du följande kommando för att visa utdatafiler som lagras på **wasb://example/data/WordCountOutput**:</span><span class="sxs-lookup"><span data-stu-id="66e4e-127">When the job is complete, use the following command to list the output files stored at **wasb://example/data/WordCountOutput**:</span></span>

        hadoop fs -ls wasb:///example/data/WordCountOutput

    <span data-ttu-id="66e4e-128">Visas två filer, **_SUCCESS** och **del-r-00000**.</span><span class="sxs-lookup"><span data-stu-id="66e4e-128">This should display two files, **_SUCCESS** and **part-r-00000**.</span></span> <span data-ttu-id="66e4e-129">Den **del-r-00000** filen innehåller utdata för jobbet.</span><span class="sxs-lookup"><span data-stu-id="66e4e-129">The **part-r-00000** file contains the output for this job.</span></span>

   > [!NOTE]
   > <span data-ttu-id="66e4e-130">Vissa MapReduce-jobb kan delas upp resultaten över flera **del-r-###** filer.</span><span class="sxs-lookup"><span data-stu-id="66e4e-130">Some MapReduce jobs may split the results across multiple **part-r-#####** files.</span></span> <span data-ttu-id="66e4e-131">I så fall använder den ### suffix för att ange ordningen för filerna.</span><span class="sxs-lookup"><span data-stu-id="66e4e-131">If so, use the ##### suffix to indicate the order of the files.</span></span>
   >
   >
5. <span data-ttu-id="66e4e-132">Om du vill visa utdata, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="66e4e-132">To view the output, use the following command:</span></span>

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    <span data-ttu-id="66e4e-133">Detta visar en lista över de ord som finns i den **wasb://example/data/gutenberg/davinci.txt** fil och hur många gånger varje ord inträffade.</span><span class="sxs-lookup"><span data-stu-id="66e4e-133">This displays a list of the words that are contained in the **wasb://example/data/gutenberg/davinci.txt** file, along with the number of times each word occured.</span></span> <span data-ttu-id="66e4e-134">Följande är ett exempel på de data som ska ingå i filen:</span><span class="sxs-lookup"><span data-stu-id="66e4e-134">The following is an example of the data that will be contained in the file:</span></span>

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <span data-ttu-id="66e4e-135"><a id="summary"></a>Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="66e4e-135"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="66e4e-136">Som du ser ger kommandot Hadoop ett enkelt sätt att köra MapReduce-jobb på ett HDInsight-kluster och sedan visa jobbutdata.</span><span class="sxs-lookup"><span data-stu-id="66e4e-136">As you can see, the Hadoop command provides an easy way to run MapReduce jobs on an HDInsight cluster and then view the job output.</span></span>

## <span data-ttu-id="66e4e-137"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66e4e-137"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="66e4e-138">Allmän information om MapReduce-jobb i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="66e4e-138">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="66e4e-139">Använda MapReduce på Hadoop och HDInsight</span><span class="sxs-lookup"><span data-stu-id="66e4e-139">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="66e4e-140">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="66e4e-140">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="66e4e-141">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="66e4e-141">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="66e4e-142">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="66e4e-142">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
