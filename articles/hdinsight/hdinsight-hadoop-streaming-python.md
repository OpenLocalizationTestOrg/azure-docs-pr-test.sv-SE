---
title: "aaaDevelop Python strömning MapReduce-jobb med HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse Python med strömmande MapReduce-jobb. Hadoop innehåller ett strömmande API för MapReduce för att skriva på andra språk än Java."
services: hdinsight
keyword: mapreduce python,python map reduce,python mapreduce
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7631d8d9-98ae-42ec-b9ec-ee3cf7e57fb3
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: a6ae3ba650b665ecc5839a4ddf5282f8ccfb6bd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-python-streaming-mapreduce-programs-for-hdinsight"></a><span data-ttu-id="3e037-104">Utveckla Python strömning MapReduce program för HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e037-104">Develop Python streaming MapReduce programs for HDInsight</span></span>

<span data-ttu-id="3e037-105">Lär dig hur toouse Python med strömmande MapReduce-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="3e037-105">Learn how toouse Python in streaming MapReduce operations.</span></span> <span data-ttu-id="3e037-106">Hadoop innehåller ett strömmande API för MapReduce som aktiverar toowrite kartan och minska funktioner på andra språk än Java.</span><span class="sxs-lookup"><span data-stu-id="3e037-106">Hadoop provides a streaming API for MapReduce that enables you toowrite map and reduce functions in languages other than Java.</span></span> <span data-ttu-id="3e037-107">hello stegen i det här dokumentet implementera hello kartan och minska komponenter i Python.</span><span class="sxs-lookup"><span data-stu-id="3e037-107">hello steps in this document implement hello Map and Reduce components in Python.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e037-108">Krav</span><span class="sxs-lookup"><span data-stu-id="3e037-108">Prerequisites</span></span>

* <span data-ttu-id="3e037-109">En Linux-baserade Hadoop på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="3e037-109">A Linux-based Hadoop on HDInsight cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="3e037-110">hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="3e037-110">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="3e037-111">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3e037-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3e037-112">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="3e037-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="3e037-113">En textredigerare</span><span class="sxs-lookup"><span data-stu-id="3e037-113">A text editor</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="3e037-114">hello textredigerare måste använda LF hello rad avslutas.</span><span class="sxs-lookup"><span data-stu-id="3e037-114">hello text editor must use LF as hello line ending.</span></span> <span data-ttu-id="3e037-115">Med hjälp av en rad avslutades av CRLF orsakar fel när du kör hello MapReduce-jobb på Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3e037-115">Using a line ending of CRLF causes errors when running hello MapReduce job on Linux-based HDInsight clusters.</span></span>

* <span data-ttu-id="3e037-116">Hej `ssh` och `scp` kommandon, eller [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)</span><span class="sxs-lookup"><span data-stu-id="3e037-116">hello `ssh` and `scp` commands, or [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)</span></span>

## <a name="word-count"></a><span data-ttu-id="3e037-117">Räkna ord</span><span class="sxs-lookup"><span data-stu-id="3e037-117">Word count</span></span>

<span data-ttu-id="3e037-118">Det här exemplet är en grundläggande ordräkning genomföras i en python en mapper och reducer.</span><span class="sxs-lookup"><span data-stu-id="3e037-118">This example is a basic word count implemented in a python a mapper and reducer.</span></span> <span data-ttu-id="3e037-119">hello mapper bryter meningar i individuella ord och hello reducer aggregerar hello ord och räknar tooproduce hello utdata.</span><span class="sxs-lookup"><span data-stu-id="3e037-119">hello mapper breaks sentences into individual words, and hello reducer aggregates hello words and counts tooproduce hello output.</span></span>

<span data-ttu-id="3e037-120">hello följande flödesschema visar vad som händer under hello kartan och minska faser.</span><span class="sxs-lookup"><span data-stu-id="3e037-120">hello following flowchart illustrates what happens during hello map and reduce phases.</span></span>

![Bild av hello mapreduce-processen](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="streaming-mapreduce"></a><span data-ttu-id="3e037-122">Strömmande MapReduce</span><span class="sxs-lookup"><span data-stu-id="3e037-122">Streaming MapReduce</span></span>

<span data-ttu-id="3e037-123">Hadoop kan toospecify en fil som innehåller hello kartan och minska logik som används av ett jobb.</span><span class="sxs-lookup"><span data-stu-id="3e037-123">Hadoop allows you toospecify a file that contains hello map and reduce logic that is used by a job.</span></span> <span data-ttu-id="3e037-124">hello särskilda krav för hello mappa och minska logik är:</span><span class="sxs-lookup"><span data-stu-id="3e037-124">hello specific requirements for hello map and reduce logic are:</span></span>

* <span data-ttu-id="3e037-125">**Inkommande**: hello kartan och minska komponenter måste läsa indata från STDIN.</span><span class="sxs-lookup"><span data-stu-id="3e037-125">**Input**: hello map and reduce components must read input data from STDIN.</span></span>
* <span data-ttu-id="3e037-126">**Utdata**: hello kartan och minska komponenter måste skriva utdata data tooSTDOUT.</span><span class="sxs-lookup"><span data-stu-id="3e037-126">**Output**: hello map and reduce components must write output data tooSTDOUT.</span></span>
* <span data-ttu-id="3e037-127">**Dataformatet**: hello data används och producerade måste vara ett nyckel/värde-par, avgränsade med semikolon fliken.</span><span class="sxs-lookup"><span data-stu-id="3e037-127">**Data format**: hello data consumed and produced must be a key/value pair, separated by a tab character.</span></span>

<span data-ttu-id="3e037-128">Python kan enkelt hantera dessa krav med hjälp av hello `sys` modulen tooread från STDIN och använda `print` tooprint tooSTDOUT.</span><span class="sxs-lookup"><span data-stu-id="3e037-128">Python can easily handle these requirements by using hello `sys` module tooread from STDIN and using `print` tooprint tooSTDOUT.</span></span> <span data-ttu-id="3e037-129">hello återstående aktivitet helt enkelt formatera hello data med en flik (`\t`) tecken mellan hello nyckel och värde.</span><span class="sxs-lookup"><span data-stu-id="3e037-129">hello remaining task is simply formatting hello data with a tab (`\t`) character between hello key and value.</span></span>

## <a name="create-hello-mapper-and-reducer"></a><span data-ttu-id="3e037-130">Skapa hello mapper och reducer</span><span class="sxs-lookup"><span data-stu-id="3e037-130">Create hello mapper and reducer</span></span>

1. <span data-ttu-id="3e037-131">Skapa en fil med namnet `mapper.py` och Använd hello efter koden som hello innehåll:</span><span class="sxs-lookup"><span data-stu-id="3e037-131">Create a file named `mapper.py` and use hello following code as hello content:</span></span>

   ```python
   #!/usr/bin/env python

   # Use hello sys module
   import sys

   # 'file' in this case is STDIN
   def read_input(file):
       # Split each line into words
       for line in file:
           yield line.split()

   def main(separator='\t'):
       # Read hello data using read_input
       data = read_input(sys.stdin)
       # Process each word returned from read_input
       for words in data:
           # Process each word
           for word in words:
               # Write tooSTDOUT
               print '%s%s%d' % (word, separator, 1)

   if __name__ == "__main__":
       main()
   ```

2. <span data-ttu-id="3e037-132">Skapa en fil med namnet **reducer.py** och Använd hello efter koden som hello innehåll:</span><span class="sxs-lookup"><span data-stu-id="3e037-132">Create a file named **reducer.py** and use hello following code as hello content:</span></span>

   ```python
   #!/usr/bin/env python

   # import modules
   from itertools import groupby
   from operator import itemgetter
   import sys

   # 'file' in this case is STDIN
   def read_mapper_output(file, separator='\t'):
       # Go through each line
       for line in file:
           # Strip out hello separator character
           yield line.rstrip().split(separator, 1)

   def main(separator='\t'):
       # Read hello data using read_mapper_output
       data = read_mapper_output(sys.stdin, separator=separator)
       # Group words and counts into 'group'
       #   Since MapReduce is a distributed process, each word
       #   may have multiple counts. 'group' will have all counts
       #   which can be retrieved using hello word as hello key.
       for current_word, group in groupby(data, itemgetter(0)):
           try:
               # For each word, pull hello count(s) for hello word
               #   from 'group' and create a total count
               total_count = sum(int(count) for current_word, count in group)
               # Write toostdout
               print "%s%s%d" % (current_word, separator, total_count)
           except ValueError:
               # Count was not a number, so do nothing
               pass

   if __name__ == "__main__":
       main()
   ```

## <a name="run-using-powershell"></a><span data-ttu-id="3e037-133">Kör med PowerShell</span><span class="sxs-lookup"><span data-stu-id="3e037-133">Run using PowerShell</span></span>

<span data-ttu-id="3e037-134">tooensure att filerna har hello rätt radbrytningar, Använd hello följande PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="3e037-134">tooensure that your files have hello right line endings, use hello following PowerShell script:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]

<span data-ttu-id="3e037-135">Använd följande PowerShell-skriptet tooupload hello filer hello, kör hello jobb och visa hello utdata:</span><span class="sxs-lookup"><span data-stu-id="3e037-135">Use hello following PowerShell script tooupload hello files, run hello job, and view hello output:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]

## <a name="run-from-an-ssh-session"></a><span data-ttu-id="3e037-136">Kör från en SSH-session</span><span class="sxs-lookup"><span data-stu-id="3e037-136">Run from an SSH session</span></span>

1. <span data-ttu-id="3e037-137">Från din utvecklingsmiljö i hello samma katalog som `mapper.py` och `reducer.py` filer, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3e037-137">From your development environment, in hello same directory as `mapper.py` and `reducer.py` files, use hello following command:</span></span>

    ```bash
    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:
    ```

    <span data-ttu-id="3e037-138">Ersätt `username` med hello SSH-användarnamn för klustret, och `clustername` med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="3e037-138">Replace `username` with hello SSH user name for your cluster, and `clustername` with hello name of your cluster.</span></span>

    <span data-ttu-id="3e037-139">Det här kommandot kopierar hello filer från lokala system hello toohello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="3e037-139">This command copies hello files from hello local system toohello head node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3e037-140">Om du har använt ett lösenord toosecure SSH-konto kan ombeds du hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="3e037-140">If you used a password toosecure your SSH account, you are prompted for hello password.</span></span> <span data-ttu-id="3e037-141">Om du använder en SSH-nyckel, kanske toouse hello `-i` parameter och hello sökvägen toohello den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="3e037-141">If you used an SSH key, you may have toouse hello `-i` parameter and hello path toohello private key.</span></span> <span data-ttu-id="3e037-142">Till exempel `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.</span><span class="sxs-lookup"><span data-stu-id="3e037-142">For example, `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.</span></span>

2. <span data-ttu-id="3e037-143">Anslut toohello kluster med hjälp av SSH:</span><span class="sxs-lookup"><span data-stu-id="3e037-143">Connect toohello cluster by using SSH:</span></span>

    ```bash
    ssh username@clustername-ssh.azurehdinsight.net`
    ```

    <span data-ttu-id="3e037-144">Mer information om finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="3e037-144">For more information on, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="3e037-145">tooensure hello mapper.py och reducer.py har hello korrigera radbrytningar genom att använda hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3e037-145">tooensure hello mapper.py and reducer.py have hello correct line endings, use hello following commands:</span></span>

    ```bash
    perl -pi -e 's/\r\n/\n/g' mapper.py
    perl -pi -e 's/\r\n/\n/g' reducer.py
    ```

4. <span data-ttu-id="3e037-146">Använd hello efter kommandot toostart hello MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="3e037-146">Use hello following command toostart hello MapReduce job.</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
    ```

    <span data-ttu-id="3e037-147">Det här kommandot har hello följande delar:</span><span class="sxs-lookup"><span data-stu-id="3e037-147">This command has hello following parts:</span></span>

   * <span data-ttu-id="3e037-148">**hadoop-streaming.jar**: används när du utför strömmande MapReduce-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="3e037-148">**hadoop-streaming.jar**: Used when performing streaming MapReduce operations.</span></span> <span data-ttu-id="3e037-149">Det gränssnitt Hadoop med hello externa MapReduce kod som du anger.</span><span class="sxs-lookup"><span data-stu-id="3e037-149">It interfaces Hadoop with hello external MapReduce code you provide.</span></span>

   * <span data-ttu-id="3e037-150">**-filer**: lägger till hello angivna filer toohello MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="3e037-150">**-files**: Adds hello specified files toohello MapReduce job.</span></span>

   * <span data-ttu-id="3e037-151">**-mapper**: talar om för Hadoop som filen toouse som hello mapper.</span><span class="sxs-lookup"><span data-stu-id="3e037-151">**-mapper**: Tells Hadoop which file toouse as hello mapper.</span></span>

   * <span data-ttu-id="3e037-152">**-reducer**: talar om för Hadoop som filen toouse som hello reducer.</span><span class="sxs-lookup"><span data-stu-id="3e037-152">**-reducer**: Tells Hadoop which file toouse as hello reducer.</span></span>

   * <span data-ttu-id="3e037-153">**-inkommande**: hello indatafil som vi ska räkna ord från.</span><span class="sxs-lookup"><span data-stu-id="3e037-153">**-input**: hello input file that we should count words from.</span></span>

   * <span data-ttu-id="3e037-154">**-utdata**: hello-katalog som hello utdata skrivs till.</span><span class="sxs-lookup"><span data-stu-id="3e037-154">**-output**: hello directory that hello output is written to.</span></span>

    <span data-ttu-id="3e037-155">Eftersom hello MapReduce-jobb fungerar, visas hello process som procenttal.</span><span class="sxs-lookup"><span data-stu-id="3e037-155">As hello MapReduce job works, hello process is displayed as percentages.</span></span>

        <span data-ttu-id="3e037-156">05-02-15 19:01:04 INFO mapreduce. Jobbet: karta 0% minska 0% 05-02-15 19:01:16 INFO mapreduce. Jobbet: karta 100% minska 0% 05-02-15 19:01:27 INFO mapreduce. Jobbet: karta 100% minska 100%</span><span class="sxs-lookup"><span data-stu-id="3e037-156">15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%</span></span>


5. <span data-ttu-id="3e037-157">tooview hello utdata, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3e037-157">tooview hello output, use hello following command:</span></span>

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    <span data-ttu-id="3e037-158">Det här kommandot visar en lista över ord och hur många gånger hello word inträffade.</span><span class="sxs-lookup"><span data-stu-id="3e037-158">This command displays a list of words and how many times hello word occurred.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e037-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e037-159">Next steps</span></span>

<span data-ttu-id="3e037-160">Nu när du har lärt dig hur toouse strömning MapRedcue jobb med HDInsight, använder du följande länkar tooexplore hello andra sätt toowork med Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3e037-160">Now that you have learned how toouse streaming MapRedcue jobs with HDInsight, use hello following links tooexplore other ways toowork with Azure HDInsight.</span></span>

* [<span data-ttu-id="3e037-161">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e037-161">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="3e037-162">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e037-162">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="3e037-163">Använda MapReduce-jobb med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e037-163">Use MapReduce jobs with HDInsight</span></span>](hdinsight-use-mapreduce.md)
