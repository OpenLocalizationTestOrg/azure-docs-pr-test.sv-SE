---
title: "Kör Hadoop MapReduce exempel på HDInsight - Azure | Microsoft Docs"
description: "Komma igång med MapReduce-exempel i jar-filer som ingår i HDInsight. Använda SSH för att ansluta till klustret och sedan använda Hadoop-kommando för att köra exemplet jobb."
keywords: hadoop exempel jar, hadoop exempel jar, hadoop mapreduce exempel, mapreduce-exempel
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e1d2a0b9-1659-4fab-921e-4a8990cbb30a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 447c07f869ff9a2a2a00089248be98e6729d6dc4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="run-the-mapreduce-examples-included-in-hdinsight"></a><span data-ttu-id="3cb53-105">Kör MapReduce-exempel som ingår i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3cb53-105">Run the MapReduce examples included in HDInsight</span></span>

[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

<span data-ttu-id="3cb53-106">Lär dig hur du kör MapReduce-exempel som ingår i Hadoop i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3cb53-106">Learn how to run the MapReduce examples included with Hadoop on HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3cb53-107">Krav</span><span class="sxs-lookup"><span data-stu-id="3cb53-107">Prerequisites</span></span>

* <span data-ttu-id="3cb53-108">**Ett HDInsight-kluster**: finns [komma igång med Hadoop med Hive i HDInsight på Linux](hdinsight-hadoop-linux-tutorial-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="3cb53-108">**An HDInsight cluster**: See [Get started using Hadoop with Hive in HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="3cb53-109">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="3cb53-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3cb53-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="3cb53-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="3cb53-111">**En SSH-klient**: Mer information finns i [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="3cb53-111">**An SSH client**: For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="the-mapreduce-examples"></a><span data-ttu-id="3cb53-112">MapReduce-exempel</span><span class="sxs-lookup"><span data-stu-id="3cb53-112">The MapReduce examples</span></span>

<span data-ttu-id="3cb53-113">**Plats**: exemplen finns på HDInsight-kluster på `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.</span><span class="sxs-lookup"><span data-stu-id="3cb53-113">**Location**: The samples are located on the HDInsight cluster at `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.</span></span>

<span data-ttu-id="3cb53-114">**Innehållet**: I följande exempel finns i det här arkivet:</span><span class="sxs-lookup"><span data-stu-id="3cb53-114">**Contents**: The following samples are contained in this archive:</span></span>

* <span data-ttu-id="3cb53-115">`aggregatewordcount`: En aggregering baserat mapreduce-program som räknas orden i indatafilerna.</span><span class="sxs-lookup"><span data-stu-id="3cb53-115">`aggregatewordcount`: An Aggregate based mapreduce program that counts the words in the input files.</span></span>
* <span data-ttu-id="3cb53-116">`aggregatewordhist`: En aggregering baserat mapreduce-program som beräknar histogram ord i indatafilerna.</span><span class="sxs-lookup"><span data-stu-id="3cb53-116">`aggregatewordhist`: An Aggregate based mapreduce program that computes the histogram of the words in the input files.</span></span>
* <span data-ttu-id="3cb53-117">`bbp`: Ett mapreduce-program som använder Bailey-Borwein-Plouffe för att beräkna exakta siffror pi.</span><span class="sxs-lookup"><span data-stu-id="3cb53-117">`bbp`: A mapreduce program that uses Bailey-Borwein-Plouffe to compute exact digits of Pi.</span></span>
* <span data-ttu-id="3cb53-118">`dbcount`: Ett exempel jobb som räknar pageview loggfilerna lagras i en databas.</span><span class="sxs-lookup"><span data-stu-id="3cb53-118">`dbcount`: An example job that counts the pageview logs stored in a database.</span></span>
* <span data-ttu-id="3cb53-119">`distbbp`: Ett mapreduce-program som använder en BBP-type-formel för att beräkna exakt bits pi.</span><span class="sxs-lookup"><span data-stu-id="3cb53-119">`distbbp`: A mapreduce program that uses a BBP-type formula to compute exact bits of Pi.</span></span>
* <span data-ttu-id="3cb53-120">`grep`: Ett mapreduce-program som räknar matchningar av en regex i indata.</span><span class="sxs-lookup"><span data-stu-id="3cb53-120">`grep`: A mapreduce program that counts the matches of a regex in the input.</span></span>
* <span data-ttu-id="3cb53-121">`join`: Ett jobb som utför en koppling över sorteras, lika partitionerad datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="3cb53-121">`join`: A job that performs a join over sorted, equally partitioned datasets.</span></span>
* <span data-ttu-id="3cb53-122">`multifilewc`: Ett jobb som räknar ord från flera filer.</span><span class="sxs-lookup"><span data-stu-id="3cb53-122">`multifilewc`: A job that counts words from several files.</span></span>
* <span data-ttu-id="3cb53-123">`pentomino`: Ett mapreduce sida vid sida om program för att hitta lösningar på problem med pentomino.</span><span class="sxs-lookup"><span data-stu-id="3cb53-123">`pentomino`: A mapreduce tile laying program to find solutions to pentomino problems.</span></span>
* <span data-ttu-id="3cb53-124">`pi`: Ett mapreduce-program som beräknar Pi med hjälp av en kvasi Monte Carlo-metoden.</span><span class="sxs-lookup"><span data-stu-id="3cb53-124">`pi`: A mapreduce program that estimates Pi using a quasi-Monte Carlo method.</span></span>
* <span data-ttu-id="3cb53-125">`randomtextwriter`: Ett mapreduce-program som skriver 10 GB av slumpmässiga textdata per nod.</span><span class="sxs-lookup"><span data-stu-id="3cb53-125">`randomtextwriter`: A mapreduce program that writes 10 GB of random textual data per node.</span></span>
* <span data-ttu-id="3cb53-126">`randomwriter`: Ett mapreduce-program som skriver 10 GB slumpmässiga data per nod.</span><span class="sxs-lookup"><span data-stu-id="3cb53-126">`randomwriter`: A mapreduce program that writes 10 GB of random data per node.</span></span>
* <span data-ttu-id="3cb53-127">`secondarysort`: Ett exempel som definierar en sekundär sortering till minska fasen.</span><span class="sxs-lookup"><span data-stu-id="3cb53-127">`secondarysort`: An example defining a secondary sort to the reduce phase.</span></span>
* <span data-ttu-id="3cb53-128">`sort`: Ett mapreduce-program som sorterar data som skrivs av slumpmässiga writer.</span><span class="sxs-lookup"><span data-stu-id="3cb53-128">`sort`: A mapreduce program that sorts the data written by the random writer.</span></span>
* <span data-ttu-id="3cb53-129">`sudoku`: En sudoku solver.</span><span class="sxs-lookup"><span data-stu-id="3cb53-129">`sudoku`: A sudoku solver.</span></span>
* <span data-ttu-id="3cb53-130">`teragen`: Skapa data för terasort.</span><span class="sxs-lookup"><span data-stu-id="3cb53-130">`teragen`: Generate data for the terasort.</span></span>
* <span data-ttu-id="3cb53-131">`terasort`: Kör terasort.</span><span class="sxs-lookup"><span data-stu-id="3cb53-131">`terasort`: Run the terasort.</span></span>
* <span data-ttu-id="3cb53-132">`teravalidate`: Kontrollera resultatet av terasort.</span><span class="sxs-lookup"><span data-stu-id="3cb53-132">`teravalidate`: Checking results of terasort.</span></span>
* <span data-ttu-id="3cb53-133">`wordcount`: Ett mapreduce-program som räknas orden i indatafilerna.</span><span class="sxs-lookup"><span data-stu-id="3cb53-133">`wordcount`: A mapreduce program that counts the words in the input files.</span></span>
* <span data-ttu-id="3cb53-134">`wordmean`: Ett mapreduce-program som räknar den genomsnittliga längden på texten i de inkommande filerna.</span><span class="sxs-lookup"><span data-stu-id="3cb53-134">`wordmean`: A mapreduce program that counts the average length of the words in the input files.</span></span>
* <span data-ttu-id="3cb53-135">`wordmedian`: Ett mapreduce-program som räknar ord i indatafilerna median längd.</span><span class="sxs-lookup"><span data-stu-id="3cb53-135">`wordmedian`: A mapreduce program that counts the median length of the words in the input files.</span></span>
* <span data-ttu-id="3cb53-136">`wordstandarddeviation`: Ett mapreduce-program som räknar standardavvikelsen för längden på texten i de inkommande filerna.</span><span class="sxs-lookup"><span data-stu-id="3cb53-136">`wordstandarddeviation`: A mapreduce program that counts the standard deviation of the length of the words in the input files.</span></span>

<span data-ttu-id="3cb53-137">**Källkoden**: källkoden för exemplen ingår i HDInsight-kluster på `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.</span><span class="sxs-lookup"><span data-stu-id="3cb53-137">**Source code**: Source code for these samples is included on the HDInsight cluster at `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.</span></span>

> [!NOTE]
> <span data-ttu-id="3cb53-138">Den `2.2.4.9-1` i sökvägen är versionen av Hortonworks Data Platform för HDInsight-kluster och kan vara olika för klustret.</span><span class="sxs-lookup"><span data-stu-id="3cb53-138">The `2.2.4.9-1` in the path is the version of the Hortonworks Data Platform for the HDInsight cluster, and may be different for your cluster.</span></span>

## <a name="run-the-wordcount-example"></a><span data-ttu-id="3cb53-139">Köra wordcount-exemplet</span><span class="sxs-lookup"><span data-stu-id="3cb53-139">Run the wordcount example</span></span>

1. <span data-ttu-id="3cb53-140">Anslut till HDInsight med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="3cb53-140">Connect to HDInsight using SSH.</span></span> <span data-ttu-id="3cb53-141">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="3cb53-141">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="3cb53-142">Från den `username@#######:~$` uppmanar, använder du följande kommando för att lista exemplen:</span><span class="sxs-lookup"><span data-stu-id="3cb53-142">From the `username@#######:~$` prompt, use the following command to list the samples:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    <span data-ttu-id="3cb53-143">Det här kommandot skapar listan med exempel från föregående avsnitt i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="3cb53-143">This command generates the list of sample from the previous section of this document.</span></span>

3. <span data-ttu-id="3cb53-144">Använd kommandot om du vill ha hjälp med ett specifikt exempel.</span><span class="sxs-lookup"><span data-stu-id="3cb53-144">Use the following command to get help on a specific sample.</span></span> <span data-ttu-id="3cb53-145">I det här fallet den **wordcount** exempel:</span><span class="sxs-lookup"><span data-stu-id="3cb53-145">In this case, the **wordcount** sample:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    <span data-ttu-id="3cb53-146">Följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="3cb53-146">You receive the following message:</span></span>

        Usage: wordcount <in> [<in>...] <out>

    <span data-ttu-id="3cb53-147">Det här meddelandet innebär att du kan ange flera indatasökvägarna för källdokument.</span><span class="sxs-lookup"><span data-stu-id="3cb53-147">This message indicates that you can provide several input paths for the source documents.</span></span> <span data-ttu-id="3cb53-148">Den slutliga sökvägen är där utdata (antal ord i källdokument) lagras.</span><span class="sxs-lookup"><span data-stu-id="3cb53-148">The final path is where the output (count of words in the source documents) is stored.</span></span>

4. <span data-ttu-id="3cb53-149">Använd följande för att räkna alla orden i den bärbara datorer för Leonardo Da Vinci, som tillhandahålls som exempeldata med ditt kluster:</span><span class="sxs-lookup"><span data-stu-id="3cb53-149">Use the following to count all words in the Notebooks of Leonardo Da Vinci, which are provided as sample data with your cluster:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    <span data-ttu-id="3cb53-150">Indata för det här jobbet har lästs från `/example/data/gutenberg/davinci.txt`.</span><span class="sxs-lookup"><span data-stu-id="3cb53-150">Input for this job is read from `/example/data/gutenberg/davinci.txt`.</span></span> <span data-ttu-id="3cb53-151">Utdata för det här exemplet är lagrad i `/example/data/davinciwordcount`.</span><span class="sxs-lookup"><span data-stu-id="3cb53-151">Output for this example is stored in `/example/data/davinciwordcount`.</span></span> <span data-ttu-id="3cb53-152">Både sökvägar finns på standardlagring för klustret, inte det lokala filsystemet.</span><span class="sxs-lookup"><span data-stu-id="3cb53-152">Both paths are located on default storage for the cluster, not the local file system.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3cb53-153">Som anges i hjälpen för wordcount-exemplet, kan du också ange flera indatafiler.</span><span class="sxs-lookup"><span data-stu-id="3cb53-153">As noted in the help for the wordcount sample, you could also specify multiple input files.</span></span> <span data-ttu-id="3cb53-154">Till exempel `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` räknas orden i både davinci.txt och ulysses.txt.</span><span class="sxs-lookup"><span data-stu-id="3cb53-154">For example, `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` would count words in both davinci.txt and ulysses.txt.</span></span>

5. <span data-ttu-id="3cb53-155">När jobbet har slutförts använder du följande kommando du visar utdata:</span><span class="sxs-lookup"><span data-stu-id="3cb53-155">Once the job completes, use the following command to view the output:</span></span>

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    <span data-ttu-id="3cb53-156">Det här kommandot sammanfogar alla utdatafiler som produceras av jobbet.</span><span class="sxs-lookup"><span data-stu-id="3cb53-156">This command concatenates all the output files produced by the job.</span></span> <span data-ttu-id="3cb53-157">Den visar utdata till konsolen.</span><span class="sxs-lookup"><span data-stu-id="3cb53-157">It displays the output to the console.</span></span> <span data-ttu-id="3cb53-158">De utdata som genereras liknar följande text:</span><span class="sxs-lookup"><span data-stu-id="3cb53-158">The output is similar to the following text:</span></span>

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    <span data-ttu-id="3cb53-159">Varje rad utgör ett ord och hur många gånger som det har uppstått i indata.</span><span class="sxs-lookup"><span data-stu-id="3cb53-159">Each line represents a word and how many times it occurred in the input data.</span></span>

## <a name="the-sudoku-example"></a><span data-ttu-id="3cb53-160">Sudoku-exempel</span><span class="sxs-lookup"><span data-stu-id="3cb53-160">The Sudoku example</span></span>

<span data-ttu-id="3cb53-161">[Sudoku](https://en.wikipedia.org/wiki/Sudoku) är logik avgöra består av nio 3 x 3 rutnät.</span><span class="sxs-lookup"><span data-stu-id="3cb53-161">[Sudoku](https://en.wikipedia.org/wiki/Sudoku) is a logic puzzle made up of nine 3x3 grids.</span></span> <span data-ttu-id="3cb53-162">Vissa celler i rutnätet har tal, medan andra är tomt och målet är att lösa för tomma celler.</span><span class="sxs-lookup"><span data-stu-id="3cb53-162">Some cells in the grid have numbers, while others are blank, and the goal is to solve for the blank cells.</span></span> <span data-ttu-id="3cb53-163">Föregående länk har mer information om problem, men det här exemplet syftar till att lösa för tomma celler.</span><span class="sxs-lookup"><span data-stu-id="3cb53-163">The previous link has more information on the puzzle, but the purpose of this sample is to solve for the blank cells.</span></span> <span data-ttu-id="3cb53-164">Så ska våra indata vara en fil som finns i följande format:</span><span class="sxs-lookup"><span data-stu-id="3cb53-164">So our input should be a file that is in the following format:</span></span>

* <span data-ttu-id="3cb53-165">Nio rader nio kolumner</span><span class="sxs-lookup"><span data-stu-id="3cb53-165">Nine rows of nine columns</span></span>
* <span data-ttu-id="3cb53-166">Varje kolumn kan innehålla antingen ett tal eller `?` (som anger en tom cell)</span><span class="sxs-lookup"><span data-stu-id="3cb53-166">Each column can contain either a number or `?` (which indicates a blank cell)</span></span>
* <span data-ttu-id="3cb53-167">Celler avgränsat med ett blanksteg</span><span class="sxs-lookup"><span data-stu-id="3cb53-167">Cells are separated by a space</span></span>

<span data-ttu-id="3cb53-168">Det finns ett visst sätt att konstruera Sudoku kanske; Du kan upprepa ett tal i en kolumn eller rad.</span><span class="sxs-lookup"><span data-stu-id="3cb53-168">There is a certain way to construct Sudoku puzzles; you can't repeat a number in a column or row.</span></span> <span data-ttu-id="3cb53-169">Det finns ett exempel på HDInsight-klustret har skapats korrekt.</span><span class="sxs-lookup"><span data-stu-id="3cb53-169">There's an example on the HDInsight cluster that is properly constructed.</span></span> <span data-ttu-id="3cb53-170">Det finns i `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` och innehåller följande text:</span><span class="sxs-lookup"><span data-stu-id="3cb53-170">It is located at `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` and contains the following text:</span></span>

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

<span data-ttu-id="3cb53-171">Om du vill köra det här exemplet problemet genom Sudoku exempel, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3cb53-171">To run this example problem through the Sudoku example, use the following command:</span></span>

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

<span data-ttu-id="3cb53-172">Resultatet liknar följande:</span><span class="sxs-lookup"><span data-stu-id="3cb53-172">The results appear similar to the following text:</span></span>

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi--example"></a><span data-ttu-id="3cb53-173">Exempel PI (π)</span><span class="sxs-lookup"><span data-stu-id="3cb53-173">Pi (π) example</span></span>

<span data-ttu-id="3cb53-174">Pi används en statistisk (kvasi Monte Carlo) metod för att uppskatta värdet för pi.</span><span class="sxs-lookup"><span data-stu-id="3cb53-174">The pi sample uses a statistical (quasi-Monte Carlo) method to estimate the value of pi.</span></span> <span data-ttu-id="3cb53-175">Punkter placeras slumpmässigt i en ruta för enheten.</span><span class="sxs-lookup"><span data-stu-id="3cb53-175">Points are placed at random in a unit square.</span></span> <span data-ttu-id="3cb53-176">Kvadraten innehåller också en cirkel.</span><span class="sxs-lookup"><span data-stu-id="3cb53-176">The square also contains a circle.</span></span> <span data-ttu-id="3cb53-177">Sannolikheten att pekar faller inom cirkeln är lika med området cirkel-pi/4.</span><span class="sxs-lookup"><span data-stu-id="3cb53-177">The probability that the points fall within the circle are equal to the area of the circle, pi/4.</span></span> <span data-ttu-id="3cb53-178">Värdet för pi beräknas från värdet för 4R.</span><span class="sxs-lookup"><span data-stu-id="3cb53-178">The value of pi can be estimated from the value of 4R.</span></span> <span data-ttu-id="3cb53-179">R är förhållandet mellan antalet punkter som finns inuti cirkeln till det totala antalet punkter som ligger inom kvadraten.</span><span class="sxs-lookup"><span data-stu-id="3cb53-179">R is the ratio of the number of points that are inside the circle to the total number of points that are within the square.</span></span> <span data-ttu-id="3cb53-180">Ju större det här exemplet används punkter bättre uppskattning är.</span><span class="sxs-lookup"><span data-stu-id="3cb53-180">The larger the sample of points used, the better the estimate is.</span></span>

<span data-ttu-id="3cb53-181">Använd följande kommando för att köra det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="3cb53-181">Use the following command to run this sample.</span></span> <span data-ttu-id="3cb53-182">Detta kommando använder 16 maps med 10 000 000 prover för att beräkna värdet för pi:</span><span class="sxs-lookup"><span data-stu-id="3cb53-182">This command uses 16 maps with 10,000,000 samples each to estimate the value of pi:</span></span>

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

<span data-ttu-id="3cb53-183">Värdet som returneras av kommandot liknar **3.14159155000000000000**.</span><span class="sxs-lookup"><span data-stu-id="3cb53-183">The value returned by this command is similar to **3.14159155000000000000**.</span></span> <span data-ttu-id="3cb53-184">Referenser är de första 10 decimalerna pi 3.1415926535.</span><span class="sxs-lookup"><span data-stu-id="3cb53-184">For references, the first 10 decimal places of pi are 3.1415926535.</span></span>

## <a name="10-gb-greysort-example"></a><span data-ttu-id="3cb53-185">10 GB Greysort exempel</span><span class="sxs-lookup"><span data-stu-id="3cb53-185">10 GB Greysort example</span></span>

<span data-ttu-id="3cb53-186">GraySort är en benchmark-sortering.</span><span class="sxs-lookup"><span data-stu-id="3cb53-186">GraySort is a benchmark sort.</span></span> <span data-ttu-id="3cb53-187">Måttet är sortera (TB per minut) som uppnås vid sortering av stora mängder data, vanligtvis en 100 TB minsta.</span><span class="sxs-lookup"><span data-stu-id="3cb53-187">The metric is the sort rate (TB/minute) that is achieved while sorting large amounts of data, usually a 100 TB minimum.</span></span>

<span data-ttu-id="3cb53-188">Det här exemplet använder en liten 10 GB data så att den kan köras relativt snabbt.</span><span class="sxs-lookup"><span data-stu-id="3cb53-188">This sample uses a modest 10 GB of data so that it can be run relatively quickly.</span></span> <span data-ttu-id="3cb53-189">MapReduce-program som utvecklats av Owen O'Malley och Arun Murthy används.</span><span class="sxs-lookup"><span data-stu-id="3cb53-189">It uses the MapReduce applications developed by Owen O'Malley and Arun Murthy.</span></span> <span data-ttu-id="3cb53-190">Dessa program vann årliga allmänna (”daytona”) terabyte sortera prestandamått i 2009 med en andel 0.578 TB per minut (100 TB 173 minuter).</span><span class="sxs-lookup"><span data-stu-id="3cb53-190">These applications won the annual general-purpose ("daytona") terabyte sort benchmark in 2009, with a rate of 0.578 TB/min (100 TB in 173 minutes).</span></span> <span data-ttu-id="3cb53-191">Mer information om den här och andra sorterings prestandamått finns i [Sortbenchmark](http://sortbenchmark.org/) plats.</span><span class="sxs-lookup"><span data-stu-id="3cb53-191">For more information on this and other sorting benchmarks, see the [Sortbenchmark](http://sortbenchmark.org/) site.</span></span>

<span data-ttu-id="3cb53-192">Det här exemplet använder tre olika MapReduce-program:</span><span class="sxs-lookup"><span data-stu-id="3cb53-192">This sample uses three sets of MapReduce programs:</span></span>

* <span data-ttu-id="3cb53-193">**TeraGen**: A MapReduce-program som skapar rader för sortering</span><span class="sxs-lookup"><span data-stu-id="3cb53-193">**TeraGen**: A MapReduce program that generates rows of data to sort</span></span>

* <span data-ttu-id="3cb53-194">**TeraSort**: exempel på indata och använder MapReduce för att sortera data i en total order</span><span class="sxs-lookup"><span data-stu-id="3cb53-194">**TeraSort**: Samples the input data and uses MapReduce to sort the data into a total order</span></span>

    <span data-ttu-id="3cb53-195">TeraSort är en standard MapReduce-sortering, förutom en anpassad partitioneraren.</span><span class="sxs-lookup"><span data-stu-id="3cb53-195">TeraSort is a standard MapReduce sort, except for a custom partitioner.</span></span> <span data-ttu-id="3cb53-196">Partitioneraren som använder en sorterad lista över N-1 provtagning nycklar som definierar nyckeln intervallet för varje minska.</span><span class="sxs-lookup"><span data-stu-id="3cb53-196">The partitioner uses a sorted list of N-1 sampled keys that define the key range for each reduce.</span></span> <span data-ttu-id="3cb53-197">I synnerhet alla nycklar sådana som exempel [i-1] < = nyckel < exempel [i] skickas till minska i.</span><span class="sxs-lookup"><span data-stu-id="3cb53-197">In particular, all keys such that sample[i-1] <= key < sample[i] are sent to reduce i.</span></span> <span data-ttu-id="3cb53-198">Den här partitioneraren garanterar att utdata för minskar i är mindre än utdata från minska i + 1.</span><span class="sxs-lookup"><span data-stu-id="3cb53-198">This partitioner guarantees that the outputs of reduce i are all less than the output of reduce i+1.</span></span>

* <span data-ttu-id="3cb53-199">**TeraValidate**: A MapReduce-program som verifierar att resultatet sorteras globalt</span><span class="sxs-lookup"><span data-stu-id="3cb53-199">**TeraValidate**: A MapReduce program that validates that the output is globally sorted</span></span>

    <span data-ttu-id="3cb53-200">Det skapar en mappning per fil i den angivna katalogen och varje mappning garanterar att varje nyckel är mindre än eller lika med det tidigare.</span><span class="sxs-lookup"><span data-stu-id="3cb53-200">It creates one map per file in the output directory, and each map ensures that each key is less than or equal to the previous one.</span></span> <span data-ttu-id="3cb53-201">Funktionen kartan genererar poster för de första och sista nycklarna för varje fil.</span><span class="sxs-lookup"><span data-stu-id="3cb53-201">The map function generates records of the first and last keys of each file.</span></span> <span data-ttu-id="3cb53-202">Funktionen minska garanterar att den första nyckeln för filen i är större än den senaste nyckeln i filen i-1.</span><span class="sxs-lookup"><span data-stu-id="3cb53-202">The reduce function ensures that the first key of file i is greater than the last key of file i-1.</span></span> <span data-ttu-id="3cb53-203">Eventuella problem rapporteras som utdata för fasen minska med nycklar som är i fel ordning.</span><span class="sxs-lookup"><span data-stu-id="3cb53-203">Any problems are reported as an output of the reduce phase, with the keys that are out of order.</span></span>

<span data-ttu-id="3cb53-204">Använd följande steg för att generera data, sortera och sedan Validera utdata:</span><span class="sxs-lookup"><span data-stu-id="3cb53-204">Use the following steps to generate data, sort, and then validate the output:</span></span>

1. <span data-ttu-id="3cb53-205">Generera en 10 GB data som lagras i HDInsight-klustret standardlagring på `/example/data/10GB-sort-input`:</span><span class="sxs-lookup"><span data-stu-id="3cb53-205">Generate 10 GB of data, which is stored to the HDInsight cluster's default storage at `/example/data/10GB-sort-input`:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    <span data-ttu-id="3cb53-206">Den `-Dmapred.map.tasks` talar om Hadoop hur många kartan uppgifter för det här jobbet.</span><span class="sxs-lookup"><span data-stu-id="3cb53-206">The `-Dmapred.map.tasks` tells Hadoop how many map tasks to use for this job.</span></span> <span data-ttu-id="3cb53-207">De två sista parametrarna instruera jobbet att skapa 10 GB data och spara den på `/example/data/10GB-sort-input`.</span><span class="sxs-lookup"><span data-stu-id="3cb53-207">The final two parameters instruct the job to create 10 GB of data and to store it at `/example/data/10GB-sort-input`.</span></span>

2. <span data-ttu-id="3cb53-208">Använd följande kommando för att sortera data:</span><span class="sxs-lookup"><span data-stu-id="3cb53-208">Use the following command to sort the data:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    <span data-ttu-id="3cb53-209">Den `-Dmapred.reduce.tasks` talar om Hadoop hur många minska uppgifter som ska användas för jobbet.</span><span class="sxs-lookup"><span data-stu-id="3cb53-209">The `-Dmapred.reduce.tasks` tells Hadoop how many reduce tasks to use for the job.</span></span> <span data-ttu-id="3cb53-210">De sista två parametrarna är bara inkommande och utgående platserna för data.</span><span class="sxs-lookup"><span data-stu-id="3cb53-210">The final two parameters are just the input and output locations for data.</span></span>

3. <span data-ttu-id="3cb53-211">Använd följande för att validera data som genereras av sorteringen:</span><span class="sxs-lookup"><span data-stu-id="3cb53-211">Use the following to validate the data generated by the sort:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a><span data-ttu-id="3cb53-212">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3cb53-212">Next steps</span></span>

<span data-ttu-id="3cb53-213">Från den här artikeln beskrivs hur du kör ingår i Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3cb53-213">From this article, you learned how to run the samples included with the Linux-based HDInsight clusters.</span></span> <span data-ttu-id="3cb53-214">Självstudier om hur du använder Pig, Hive och MapReduce med HDInsight finns i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3cb53-214">For tutorials about using Pig, Hive, and MapReduce with HDInsight, see the following topics:</span></span>

* <span data-ttu-id="3cb53-215">[Använda Pig med Hadoop i HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="3cb53-215">[Use Pig with Hadoop on HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="3cb53-216">[Använda Hive med Hadoop i HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="3cb53-216">[Use Hive with Hadoop on HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="3cb53-217">[Använda MapReduce med Hadoop i HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="3cb53-217">[Use MapReduce with Hadoop on HDInsight][hdinsight-use-mapreduce]</span></span>

[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
