---
title: "Utveckla Python strömning MapReduce-jobb med HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur du använder Python i strömning MapReduce-jobb. Hadoop innehåller ett strömmande API för MapReduce för att skriva på andra språk än Java."
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
ms.openlocfilehash: b86605c49291a99f49c4b2841d46324cfd0db56d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="develop-python-streaming-mapreduce-programs-for-hdinsight"></a><span data-ttu-id="be25d-104">Utveckla Python strömning MapReduce program för HDInsight</span><span class="sxs-lookup"><span data-stu-id="be25d-104">Develop Python streaming MapReduce programs for HDInsight</span></span>

<span data-ttu-id="be25d-105">Lär dig hur du använder Python i strömning MapReduce åtgärder.</span><span class="sxs-lookup"><span data-stu-id="be25d-105">Learn how to use Python in streaming MapReduce operations.</span></span> <span data-ttu-id="be25d-106">Hadoop innehåller ett strömmande API för MapReduce där du kan skriva kartan och minska funktioner på andra språk än Java.</span><span class="sxs-lookup"><span data-stu-id="be25d-106">Hadoop provides a streaming API for MapReduce that enables you to write map and reduce functions in languages other than Java.</span></span> <span data-ttu-id="be25d-107">Stegen i det här dokumentet implementera kartan och minska komponenter i Python.</span><span class="sxs-lookup"><span data-stu-id="be25d-107">The steps in this document implement the Map and Reduce components in Python.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="be25d-108">Krav</span><span class="sxs-lookup"><span data-stu-id="be25d-108">Prerequisites</span></span>

* <span data-ttu-id="be25d-109">En Linux-baserade Hadoop på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="be25d-109">A Linux-based Hadoop on HDInsight cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="be25d-110">Stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="be25d-110">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="be25d-111">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="be25d-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="be25d-112">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="be25d-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="be25d-113">En textredigerare</span><span class="sxs-lookup"><span data-stu-id="be25d-113">A text editor</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="be25d-114">Textredigeraren måste använda LF rad avslutas.</span><span class="sxs-lookup"><span data-stu-id="be25d-114">The text editor must use LF as the line ending.</span></span> <span data-ttu-id="be25d-115">Med hjälp av en rad avslutades av CRLF orsakar fel när MapReduce-jobbet körs på Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="be25d-115">Using a line ending of CRLF causes errors when running the MapReduce job on Linux-based HDInsight clusters.</span></span>

* <span data-ttu-id="be25d-116">Den `ssh` och `scp` kommandon, eller [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)</span><span class="sxs-lookup"><span data-stu-id="be25d-116">The `ssh` and `scp` commands, or [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)</span></span>

## <a name="word-count"></a><span data-ttu-id="be25d-117">Räkna ord</span><span class="sxs-lookup"><span data-stu-id="be25d-117">Word count</span></span>

<span data-ttu-id="be25d-118">Det här exemplet är en grundläggande ordräkning genomföras i en python en mapper och reducer.</span><span class="sxs-lookup"><span data-stu-id="be25d-118">This example is a basic word count implemented in a python a mapper and reducer.</span></span> <span data-ttu-id="be25d-119">Mapparen bryter meningar i individuella ord och reducer aggregerar orden och räknar om du vill generera utdata.</span><span class="sxs-lookup"><span data-stu-id="be25d-119">The mapper breaks sentences into individual words, and the reducer aggregates the words and counts to produce the output.</span></span>

<span data-ttu-id="be25d-120">I följande flödesschema visar vad som händer under kartan och minska faser.</span><span class="sxs-lookup"><span data-stu-id="be25d-120">The following flowchart illustrates what happens during the map and reduce phases.</span></span>

![Bild av mapreduce-processen](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="streaming-mapreduce"></a><span data-ttu-id="be25d-122">Strömmande MapReduce</span><span class="sxs-lookup"><span data-stu-id="be25d-122">Streaming MapReduce</span></span>

<span data-ttu-id="be25d-123">Hadoop kan du ange en fil som innehåller kartans och minska logik som används av ett jobb.</span><span class="sxs-lookup"><span data-stu-id="be25d-123">Hadoop allows you to specify a file that contains the map and reduce logic that is used by a job.</span></span> <span data-ttu-id="be25d-124">Särskilda krav för kartan och minska logik är:</span><span class="sxs-lookup"><span data-stu-id="be25d-124">The specific requirements for the map and reduce logic are:</span></span>

* <span data-ttu-id="be25d-125">**Inkommande**: kartan och minska komponenter måste läsa indata från STDIN.</span><span class="sxs-lookup"><span data-stu-id="be25d-125">**Input**: The map and reduce components must read input data from STDIN.</span></span>
* <span data-ttu-id="be25d-126">**Utdata**: kartan och minska komponenter måste skriva utdata till STDOUT.</span><span class="sxs-lookup"><span data-stu-id="be25d-126">**Output**: The map and reduce components must write output data to STDOUT.</span></span>
* <span data-ttu-id="be25d-127">**Dataformatet**: data används och producerade måste vara ett nyckel/värde-par, avgränsade med semikolon fliken.</span><span class="sxs-lookup"><span data-stu-id="be25d-127">**Data format**: The data consumed and produced must be a key/value pair, separated by a tab character.</span></span>

<span data-ttu-id="be25d-128">Python kan enkelt hantera dessa krav med hjälp av den `sys` modulen att läsa från STDIN och använder `print` skriva ut till STDOUT.</span><span class="sxs-lookup"><span data-stu-id="be25d-128">Python can easily handle these requirements by using the `sys` module to read from STDIN and using `print` to print to STDOUT.</span></span> <span data-ttu-id="be25d-129">Den sista aktiviteten bara formaterar data med en flik (`\t`) tecken mellan nyckel och värde.</span><span class="sxs-lookup"><span data-stu-id="be25d-129">The remaining task is simply formatting the data with a tab (`\t`) character between the key and value.</span></span>

## <a name="create-the-mapper-and-reducer"></a><span data-ttu-id="be25d-130">Skapa mapper och reducer</span><span class="sxs-lookup"><span data-stu-id="be25d-130">Create the mapper and reducer</span></span>

1. <span data-ttu-id="be25d-131">Skapa en fil med namnet `mapper.py` och använda följande kod som innehållet:</span><span class="sxs-lookup"><span data-stu-id="be25d-131">Create a file named `mapper.py` and use the following code as the content:</span></span>

   ```python
   #!/usr/bin/env python

   # Use the sys module
   import sys

   # 'file' in this case is STDIN
   def read_input(file):
       # Split each line into words
       for line in file:
           yield line.split()

   def main(separator='\t'):
       # Read the data using read_input
       data = read_input(sys.stdin)
       # Process each word returned from read_input
       for words in data:
           # Process each word
           for word in words:
               # Write to STDOUT
               print '%s%s%d' % (word, separator, 1)

   if __name__ == "__main__":
       main()
   ```

2. <span data-ttu-id="be25d-132">Skapa en fil med namnet **reducer.py** och använda följande kod som innehållet:</span><span class="sxs-lookup"><span data-stu-id="be25d-132">Create a file named **reducer.py** and use the following code as the content:</span></span>

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
           # Strip out the separator character
           yield line.rstrip().split(separator, 1)

   def main(separator='\t'):
       # Read the data using read_mapper_output
       data = read_mapper_output(sys.stdin, separator=separator)
       # Group words and counts into 'group'
       #   Since MapReduce is a distributed process, each word
       #   may have multiple counts. 'group' will have all counts
       #   which can be retrieved using the word as the key.
       for current_word, group in groupby(data, itemgetter(0)):
           try:
               # For each word, pull the count(s) for the word
               #   from 'group' and create a total count
               total_count = sum(int(count) for current_word, count in group)
               # Write to stdout
               print "%s%s%d" % (current_word, separator, total_count)
           except ValueError:
               # Count was not a number, so do nothing
               pass

   if __name__ == "__main__":
       main()
   ```

## <a name="run-using-powershell"></a><span data-ttu-id="be25d-133">Kör med PowerShell</span><span class="sxs-lookup"><span data-stu-id="be25d-133">Run using PowerShell</span></span>

<span data-ttu-id="be25d-134">Använd följande PowerShell-skript för att säkerställa att dina filer har rätt radbrytningar:</span><span class="sxs-lookup"><span data-stu-id="be25d-134">To ensure that your files have the right line endings, use the following PowerShell script:</span></span>

<span data-ttu-id="be25d-135">[!code-powershell[huvudsakliga](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]</span><span class="sxs-lookup"><span data-stu-id="be25d-135">[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]</span></span>

<span data-ttu-id="be25d-136">Använd följande PowerShell-skript för att överföra filer, kör jobbet och visar utdata:</span><span class="sxs-lookup"><span data-stu-id="be25d-136">Use the following PowerShell script to upload the files, run the job, and view the output:</span></span>

<span data-ttu-id="be25d-137">[!code-powershell[huvudsakliga](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]</span><span class="sxs-lookup"><span data-stu-id="be25d-137">[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]</span></span>

## <a name="run-from-an-ssh-session"></a><span data-ttu-id="be25d-138">Kör från en SSH-session</span><span class="sxs-lookup"><span data-stu-id="be25d-138">Run from an SSH session</span></span>

1. <span data-ttu-id="be25d-139">Från din utvecklingsmiljö i samma katalog som `mapper.py` och `reducer.py` filer, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="be25d-139">From your development environment, in the same directory as `mapper.py` and `reducer.py` files, use the following command:</span></span>

    ```bash
    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:
    ```

    <span data-ttu-id="be25d-140">Ersätt `username` med SSH-användarnamn för klustret, och `clustername` med namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="be25d-140">Replace `username` with the SSH user name for your cluster, and `clustername` with the name of your cluster.</span></span>

    <span data-ttu-id="be25d-141">Det här kommandot kopieras filerna från det lokala systemet till huvudnod.</span><span class="sxs-lookup"><span data-stu-id="be25d-141">This command copies the files from the local system to the head node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="be25d-142">Om du använde ett lösenord för att skydda ditt konto med SSH, uppmanas för lösenordet.</span><span class="sxs-lookup"><span data-stu-id="be25d-142">If you used a password to secure your SSH account, you are prompted for the password.</span></span> <span data-ttu-id="be25d-143">Om du använder en SSH-nyckel måste du kanske använda den `-i` parametern och sökvägen till den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="be25d-143">If you used an SSH key, you may have to use the `-i` parameter and the path to the private key.</span></span> <span data-ttu-id="be25d-144">Till exempel `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.</span><span class="sxs-lookup"><span data-stu-id="be25d-144">For example, `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.</span></span>

2. <span data-ttu-id="be25d-145">Anslut till klustret med hjälp av SSH:</span><span class="sxs-lookup"><span data-stu-id="be25d-145">Connect to the cluster by using SSH:</span></span>

    ```bash
    ssh username@clustername-ssh.azurehdinsight.net`
    ```

    <span data-ttu-id="be25d-146">Mer information om finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="be25d-146">For more information on, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="be25d-147">För att säkerställa mapper.py och reducer.py har rätt radbrytningar, använder du följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="be25d-147">To ensure the mapper.py and reducer.py have the correct line endings, use the following commands:</span></span>

    ```bash
    perl -pi -e 's/\r\n/\n/g' mapper.py
    perl -pi -e 's/\r\n/\n/g' reducer.py
    ```

4. <span data-ttu-id="be25d-148">Använd följande kommando för att starta MapReduce-jobbet.</span><span class="sxs-lookup"><span data-stu-id="be25d-148">Use the following command to start the MapReduce job.</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
    ```

    <span data-ttu-id="be25d-149">Det här kommandot har följande delar:</span><span class="sxs-lookup"><span data-stu-id="be25d-149">This command has the following parts:</span></span>

   * <span data-ttu-id="be25d-150">**hadoop-streaming.jar**: används när du utför strömmande MapReduce-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="be25d-150">**hadoop-streaming.jar**: Used when performing streaming MapReduce operations.</span></span> <span data-ttu-id="be25d-151">Det gränssnitt Hadoop med den externa MapReduce-kod som du anger.</span><span class="sxs-lookup"><span data-stu-id="be25d-151">It interfaces Hadoop with the external MapReduce code you provide.</span></span>

   * <span data-ttu-id="be25d-152">**-filer**: lägger till de angivna filerna till MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="be25d-152">**-files**: Adds the specified files to the MapReduce job.</span></span>

   * <span data-ttu-id="be25d-153">**-mapper**: Anger vilken fil som ska användas som mapparen för Hadoop.</span><span class="sxs-lookup"><span data-stu-id="be25d-153">**-mapper**: Tells Hadoop which file to use as the mapper.</span></span>

   * <span data-ttu-id="be25d-154">**-reducer**: Anger vilken fil som ska användas som reducer för Hadoop.</span><span class="sxs-lookup"><span data-stu-id="be25d-154">**-reducer**: Tells Hadoop which file to use as the reducer.</span></span>

   * <span data-ttu-id="be25d-155">**-inkommande**: indatafilen som vi ska räkna ord från.</span><span class="sxs-lookup"><span data-stu-id="be25d-155">**-input**: The input file that we should count words from.</span></span>

   * <span data-ttu-id="be25d-156">**-utdata**: katalogen som utdata skrivs till.</span><span class="sxs-lookup"><span data-stu-id="be25d-156">**-output**: The directory that the output is written to.</span></span>

    <span data-ttu-id="be25d-157">Eftersom MapReduce-jobb fungerar, visas processen som procenttal.</span><span class="sxs-lookup"><span data-stu-id="be25d-157">As the MapReduce job works, the process is displayed as percentages.</span></span>

        <span data-ttu-id="be25d-158">05-02-15 19:01:04 INFO mapreduce. Jobbet: karta 0% minska 0% 05-02-15 19:01:16 INFO mapreduce. Jobbet: karta 100% minska 0% 05-02-15 19:01:27 INFO mapreduce. Jobbet: karta 100% minska 100%</span><span class="sxs-lookup"><span data-stu-id="be25d-158">15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%</span></span>


5. <span data-ttu-id="be25d-159">Om du vill visa utdata, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="be25d-159">To view the output, use the following command:</span></span>

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    <span data-ttu-id="be25d-160">Det här kommandot visar en lista över ord och hur många gånger ordet inträffade.</span><span class="sxs-lookup"><span data-stu-id="be25d-160">This command displays a list of words and how many times the word occurred.</span></span>

## <a name="next-steps"></a><span data-ttu-id="be25d-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="be25d-161">Next steps</span></span>

<span data-ttu-id="be25d-162">Nu när du har lärt dig hur du använder strömmande MapRedcue jobb med HDInsight, Använd följande länkar för att undersöka andra sätt att arbeta med Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="be25d-162">Now that you have learned how to use streaming MapRedcue jobs with HDInsight, use the following links to explore other ways to work with Azure HDInsight.</span></span>

* [<span data-ttu-id="be25d-163">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="be25d-163">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="be25d-164">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="be25d-164">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="be25d-165">Använda MapReduce-jobb med HDInsight</span><span class="sxs-lookup"><span data-stu-id="be25d-165">Use MapReduce jobs with HDInsight</span></span>](hdinsight-use-mapreduce.md)
