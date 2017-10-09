---
title: "aaaRun Hadoop MapReduce exempel på HDInsight - Azure | Microsoft Docs"
description: "Komma igång med MapReduce-exempel i jar-filer som ingår i HDInsight. Använd SSH tooconnect toohello kluster och sedan använda hello Hadoop kommandot toorun exempeljobb."
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
ms.openlocfilehash: 7a16bbd51eb17570fcaa3b1e0f5990fa889c106a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-hello-mapreduce-examples-included-in-hdinsight"></a><span data-ttu-id="17557-105">Kör hello MapReduce exempel ingår i HDInsight</span><span class="sxs-lookup"><span data-stu-id="17557-105">Run hello MapReduce examples included in HDInsight</span></span>

[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

<span data-ttu-id="17557-106">Lär dig hur toorun hello MapReduce exempel ingår i Hadoop i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="17557-106">Learn how toorun hello MapReduce examples included with Hadoop on HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17557-107">Krav</span><span class="sxs-lookup"><span data-stu-id="17557-107">Prerequisites</span></span>

* <span data-ttu-id="17557-108">**Ett HDInsight-kluster**: finns [komma igång med Hadoop med Hive i HDInsight på Linux](hdinsight-hadoop-linux-tutorial-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="17557-108">**An HDInsight cluster**: See [Get started using Hadoop with Hive in HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="17557-109">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="17557-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="17557-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="17557-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="17557-111">**En SSH-klient**: Mer information finns i [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="17557-111">**An SSH client**: For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="hello-mapreduce-examples"></a><span data-ttu-id="17557-112">Hej MapReduce-exempel</span><span class="sxs-lookup"><span data-stu-id="17557-112">hello MapReduce examples</span></span>

<span data-ttu-id="17557-113">**Plats**: hello-exempel finns på hello HDInsight-kluster på `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.</span><span class="sxs-lookup"><span data-stu-id="17557-113">**Location**: hello samples are located on hello HDInsight cluster at `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.</span></span>

<span data-ttu-id="17557-114">**Innehållet**: hello följande exempel finns i det här arkivet:</span><span class="sxs-lookup"><span data-stu-id="17557-114">**Contents**: hello following samples are contained in this archive:</span></span>

* <span data-ttu-id="17557-115">`aggregatewordcount`: En aggregering baserat mapreduce-program som räknar hello ord i hello indatafiler.</span><span class="sxs-lookup"><span data-stu-id="17557-115">`aggregatewordcount`: An Aggregate based mapreduce program that counts hello words in hello input files.</span></span>
* <span data-ttu-id="17557-116">`aggregatewordhist`: En aggregering baserat mapreduce-program som beräknar hello histogram hello ord i hello indatafiler.</span><span class="sxs-lookup"><span data-stu-id="17557-116">`aggregatewordhist`: An Aggregate based mapreduce program that computes hello histogram of hello words in hello input files.</span></span>
* <span data-ttu-id="17557-117">`bbp`: Ett mapreduce-program som använder Bailey-Borwein-Plouffe toocompute exakta siffror pi.</span><span class="sxs-lookup"><span data-stu-id="17557-117">`bbp`: A mapreduce program that uses Bailey-Borwein-Plouffe toocompute exact digits of Pi.</span></span>
* <span data-ttu-id="17557-118">`dbcount`: Ett exempel jobb som räknar hello pageview loggar lagras i en databas.</span><span class="sxs-lookup"><span data-stu-id="17557-118">`dbcount`: An example job that counts hello pageview logs stored in a database.</span></span>
* <span data-ttu-id="17557-119">`distbbp`: Ett mapreduce-program som använder en typ av BBP formeln toocompute exakt bits pi.</span><span class="sxs-lookup"><span data-stu-id="17557-119">`distbbp`: A mapreduce program that uses a BBP-type formula toocompute exact bits of Pi.</span></span>
* <span data-ttu-id="17557-120">`grep`: Ett mapreduce-program som räknar hello matchar av en regex i hello indata.</span><span class="sxs-lookup"><span data-stu-id="17557-120">`grep`: A mapreduce program that counts hello matches of a regex in hello input.</span></span>
* <span data-ttu-id="17557-121">`join`: Ett jobb som utför en koppling över sorteras, lika partitionerad datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="17557-121">`join`: A job that performs a join over sorted, equally partitioned datasets.</span></span>
* <span data-ttu-id="17557-122">`multifilewc`: Ett jobb som räknar ord från flera filer.</span><span class="sxs-lookup"><span data-stu-id="17557-122">`multifilewc`: A job that counts words from several files.</span></span>
* <span data-ttu-id="17557-123">`pentomino`: En mapreduce sida vid sida om programmet toofind lösningar toopentomino problem.</span><span class="sxs-lookup"><span data-stu-id="17557-123">`pentomino`: A mapreduce tile laying program toofind solutions toopentomino problems.</span></span>
* <span data-ttu-id="17557-124">`pi`: Ett mapreduce-program som beräknar Pi med hjälp av en kvasi Monte Carlo-metoden.</span><span class="sxs-lookup"><span data-stu-id="17557-124">`pi`: A mapreduce program that estimates Pi using a quasi-Monte Carlo method.</span></span>
* <span data-ttu-id="17557-125">`randomtextwriter`: Ett mapreduce-program som skriver 10 GB av slumpmässiga textdata per nod.</span><span class="sxs-lookup"><span data-stu-id="17557-125">`randomtextwriter`: A mapreduce program that writes 10 GB of random textual data per node.</span></span>
* <span data-ttu-id="17557-126">`randomwriter`: Ett mapreduce-program som skriver 10 GB slumpmässiga data per nod.</span><span class="sxs-lookup"><span data-stu-id="17557-126">`randomwriter`: A mapreduce program that writes 10 GB of random data per node.</span></span>
* <span data-ttu-id="17557-127">`secondarysort`: Ett exempel som definierar en sekundär sortera toohello minska fasen.</span><span class="sxs-lookup"><span data-stu-id="17557-127">`secondarysort`: An example defining a secondary sort toohello reduce phase.</span></span>
* <span data-ttu-id="17557-128">`sort`: Ett mapreduce-program som sorterar hello data som skrivs av slumpmässiga hello-skrivaren.</span><span class="sxs-lookup"><span data-stu-id="17557-128">`sort`: A mapreduce program that sorts hello data written by hello random writer.</span></span>
* <span data-ttu-id="17557-129">`sudoku`: En sudoku solver.</span><span class="sxs-lookup"><span data-stu-id="17557-129">`sudoku`: A sudoku solver.</span></span>
* <span data-ttu-id="17557-130">`teragen`: Skapa data för hello terasort.</span><span class="sxs-lookup"><span data-stu-id="17557-130">`teragen`: Generate data for hello terasort.</span></span>
* <span data-ttu-id="17557-131">`terasort`: Kör hello terasort.</span><span class="sxs-lookup"><span data-stu-id="17557-131">`terasort`: Run hello terasort.</span></span>
* <span data-ttu-id="17557-132">`teravalidate`: Kontrollera resultatet av terasort.</span><span class="sxs-lookup"><span data-stu-id="17557-132">`teravalidate`: Checking results of terasort.</span></span>
* <span data-ttu-id="17557-133">`wordcount`: Ett mapreduce-program som räknar hello ord i hello indatafiler.</span><span class="sxs-lookup"><span data-stu-id="17557-133">`wordcount`: A mapreduce program that counts hello words in hello input files.</span></span>
* <span data-ttu-id="17557-134">`wordmean`: Ett mapreduce-program som räknar hello genomsnittliga längden på hello ord i hello indatafiler.</span><span class="sxs-lookup"><span data-stu-id="17557-134">`wordmean`: A mapreduce program that counts hello average length of hello words in hello input files.</span></span>
* <span data-ttu-id="17557-135">`wordmedian`: Ett mapreduce-program som räknar hello median längd hello ord i hello indatafiler.</span><span class="sxs-lookup"><span data-stu-id="17557-135">`wordmedian`: A mapreduce program that counts hello median length of hello words in hello input files.</span></span>
* <span data-ttu-id="17557-136">`wordstandarddeviation`: Ett mapreduce-program som räknar hello standardavvikelsen för hello längd hello ord i hello indatafiler.</span><span class="sxs-lookup"><span data-stu-id="17557-136">`wordstandarddeviation`: A mapreduce program that counts hello standard deviation of hello length of hello words in hello input files.</span></span>

<span data-ttu-id="17557-137">**Källkoden**: källkoden för exemplen ingår i HDInsight-kluster på hello `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.</span><span class="sxs-lookup"><span data-stu-id="17557-137">**Source code**: Source code for these samples is included on hello HDInsight cluster at `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.</span></span>

> [!NOTE]
> <span data-ttu-id="17557-138">Hej `2.2.4.9-1` i hello sökväg är hello version av hello Hortonworks Data Platform för hello HDInsight-kluster och kan vara olika för klustret.</span><span class="sxs-lookup"><span data-stu-id="17557-138">hello `2.2.4.9-1` in hello path is hello version of hello Hortonworks Data Platform for hello HDInsight cluster, and may be different for your cluster.</span></span>

## <a name="run-hello-wordcount-example"></a><span data-ttu-id="17557-139">Kör hello wordcount-exemplet</span><span class="sxs-lookup"><span data-stu-id="17557-139">Run hello wordcount example</span></span>

1. <span data-ttu-id="17557-140">Ansluta tooHDInsight via SSH.</span><span class="sxs-lookup"><span data-stu-id="17557-140">Connect tooHDInsight using SSH.</span></span> <span data-ttu-id="17557-141">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="17557-141">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="17557-142">Från hello `username@#######:~$` uppmanar, Använd följande kommandoexempel toolist hello hello:</span><span class="sxs-lookup"><span data-stu-id="17557-142">From hello `username@#######:~$` prompt, use hello following command toolist hello samples:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    <span data-ttu-id="17557-143">Det här kommandot genererar hello lista med exempel från hello föregående avsnitt i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="17557-143">This command generates hello list of sample from hello previous section of this document.</span></span>

3. <span data-ttu-id="17557-144">Använd hello följande kommando tooget hjälp med ett specifikt exempel.</span><span class="sxs-lookup"><span data-stu-id="17557-144">Use hello following command tooget help on a specific sample.</span></span> <span data-ttu-id="17557-145">I det här fallet hello **wordcount** exempel:</span><span class="sxs-lookup"><span data-stu-id="17557-145">In this case, hello **wordcount** sample:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    <span data-ttu-id="17557-146">Hello följande meddelande visas:</span><span class="sxs-lookup"><span data-stu-id="17557-146">You receive hello following message:</span></span>

        Usage: wordcount <in> [<in>...] <out>

    <span data-ttu-id="17557-147">Det här meddelandet innebär att du kan ange flera indatasökvägarna för hello källdokument.</span><span class="sxs-lookup"><span data-stu-id="17557-147">This message indicates that you can provide several input paths for hello source documents.</span></span> <span data-ttu-id="17557-148">hello slutliga sökvägen är där hello utdata (antal ord i hello källdokument) lagras.</span><span class="sxs-lookup"><span data-stu-id="17557-148">hello final path is where hello output (count of words in hello source documents) is stored.</span></span>

4. <span data-ttu-id="17557-149">Använd följande toocount hello alla ord i hello anteckningsböcker av Leonardo Da Vinci, som tillhandahålls som exempeldata med ditt kluster:</span><span class="sxs-lookup"><span data-stu-id="17557-149">Use hello following toocount all words in hello Notebooks of Leonardo Da Vinci, which are provided as sample data with your cluster:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    <span data-ttu-id="17557-150">Indata för det här jobbet har lästs från `/example/data/gutenberg/davinci.txt`.</span><span class="sxs-lookup"><span data-stu-id="17557-150">Input for this job is read from `/example/data/gutenberg/davinci.txt`.</span></span> <span data-ttu-id="17557-151">Utdata för det här exemplet är lagrad i `/example/data/davinciwordcount`.</span><span class="sxs-lookup"><span data-stu-id="17557-151">Output for this example is stored in `/example/data/davinciwordcount`.</span></span> <span data-ttu-id="17557-152">Både sökvägar finns på standardlagring för hello kluster inte hello som lokala filsystem.</span><span class="sxs-lookup"><span data-stu-id="17557-152">Both paths are located on default storage for hello cluster, not hello local file system.</span></span>

   > [!NOTE]
   > <span data-ttu-id="17557-153">Enligt beskrivningen i hello hjälp för hello wordcount-exemplet, kan du också ange flera indatafiler.</span><span class="sxs-lookup"><span data-stu-id="17557-153">As noted in hello help for hello wordcount sample, you could also specify multiple input files.</span></span> <span data-ttu-id="17557-154">Till exempel `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` räknas orden i både davinci.txt och ulysses.txt.</span><span class="sxs-lookup"><span data-stu-id="17557-154">For example, `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` would count words in both davinci.txt and ulysses.txt.</span></span>

5. <span data-ttu-id="17557-155">När hello jobbet är slutfört, använder du följande kommandoutdata tooview hello hello:</span><span class="sxs-lookup"><span data-stu-id="17557-155">Once hello job completes, use hello following command tooview hello output:</span></span>

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    <span data-ttu-id="17557-156">Det här kommandot sammanfogar alla hello utdatafiler som produceras av hello jobb.</span><span class="sxs-lookup"><span data-stu-id="17557-156">This command concatenates all hello output files produced by hello job.</span></span> <span data-ttu-id="17557-157">Den visar hello utdata toohello konsolen.</span><span class="sxs-lookup"><span data-stu-id="17557-157">It displays hello output toohello console.</span></span> <span data-ttu-id="17557-158">hello utdata är liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="17557-158">hello output is similar toohello following text:</span></span>

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    <span data-ttu-id="17557-159">Varje rad utgör ett ord och hur många gånger som det har uppstått i hello indata.</span><span class="sxs-lookup"><span data-stu-id="17557-159">Each line represents a word and how many times it occurred in hello input data.</span></span>

## <a name="hello-sudoku-example"></a><span data-ttu-id="17557-160">Hej Sudoku exempel</span><span class="sxs-lookup"><span data-stu-id="17557-160">hello Sudoku example</span></span>

<span data-ttu-id="17557-161">[Sudoku](https://en.wikipedia.org/wiki/Sudoku) är logik avgöra består av nio 3 x 3 rutnät.</span><span class="sxs-lookup"><span data-stu-id="17557-161">[Sudoku](https://en.wikipedia.org/wiki/Sudoku) is a logic puzzle made up of nine 3x3 grids.</span></span> <span data-ttu-id="17557-162">Vissa celler i rutnätet hello har tal, medan andra är tomt och hello målet är toosolve för hello tomma celler.</span><span class="sxs-lookup"><span data-stu-id="17557-162">Some cells in hello grid have numbers, while others are blank, and hello goal is toosolve for hello blank cells.</span></span> <span data-ttu-id="17557-163">hello föregående länk har mer information om hello problem, men hello syftet med det här exemplet är toosolve för hello tomma celler.</span><span class="sxs-lookup"><span data-stu-id="17557-163">hello previous link has more information on hello puzzle, but hello purpose of this sample is toosolve for hello blank cells.</span></span> <span data-ttu-id="17557-164">Så våra indata ska vara en fil som finns i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="17557-164">So our input should be a file that is in hello following format:</span></span>

* <span data-ttu-id="17557-165">Nio rader nio kolumner</span><span class="sxs-lookup"><span data-stu-id="17557-165">Nine rows of nine columns</span></span>
* <span data-ttu-id="17557-166">Varje kolumn kan innehålla antingen ett tal eller `?` (som anger en tom cell)</span><span class="sxs-lookup"><span data-stu-id="17557-166">Each column can contain either a number or `?` (which indicates a blank cell)</span></span>
* <span data-ttu-id="17557-167">Celler avgränsat med ett blanksteg</span><span class="sxs-lookup"><span data-stu-id="17557-167">Cells are separated by a space</span></span>

<span data-ttu-id="17557-168">Det finns ett visst sätt tooconstruct Sudoku kanske; Du kan upprepa ett tal i en kolumn eller rad.</span><span class="sxs-lookup"><span data-stu-id="17557-168">There is a certain way tooconstruct Sudoku puzzles; you can't repeat a number in a column or row.</span></span> <span data-ttu-id="17557-169">Det finns ett exempel på hello HDInsight-kluster som är angett på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="17557-169">There's an example on hello HDInsight cluster that is properly constructed.</span></span> <span data-ttu-id="17557-170">Det finns i `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` och innehåller hello följande text:</span><span class="sxs-lookup"><span data-stu-id="17557-170">It is located at `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` and contains hello following text:</span></span>

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

<span data-ttu-id="17557-171">toorun problemet exempel via hello Sudoku exempelvis använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="17557-171">toorun this example problem through hello Sudoku example, use hello following command:</span></span>

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

<span data-ttu-id="17557-172">hello sökresultaten liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="17557-172">hello results appear similar toohello following text:</span></span>

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi--example"></a><span data-ttu-id="17557-173">Exempel PI (π)</span><span class="sxs-lookup"><span data-stu-id="17557-173">Pi (π) example</span></span>

<span data-ttu-id="17557-174">hello pi används en statistisk (kvasi Monte Carlo) metoden tooestimate hello värdet för pi.</span><span class="sxs-lookup"><span data-stu-id="17557-174">hello pi sample uses a statistical (quasi-Monte Carlo) method tooestimate hello value of pi.</span></span> <span data-ttu-id="17557-175">Punkter placeras slumpmässigt i en ruta för enheten.</span><span class="sxs-lookup"><span data-stu-id="17557-175">Points are placed at random in a unit square.</span></span> <span data-ttu-id="17557-176">hello fyrkant innehåller också en cirkel.</span><span class="sxs-lookup"><span data-stu-id="17557-176">hello square also contains a circle.</span></span> <span data-ttu-id="17557-177">hello sannolikheten att hello punkter faller inom hello cirkel är lika toohello område i hello cirkel pi/4.</span><span class="sxs-lookup"><span data-stu-id="17557-177">hello probability that hello points fall within hello circle are equal toohello area of hello circle, pi/4.</span></span> <span data-ttu-id="17557-178">hello-värdet för pi beräknas från 4R hello värdet.</span><span class="sxs-lookup"><span data-stu-id="17557-178">hello value of pi can be estimated from hello value of 4R.</span></span> <span data-ttu-id="17557-179">R är hello förhållandet mellan hello antalet punkter som finns inuti hello cirkel toohello totala antalet punkter som ligger inom hello ruta.</span><span class="sxs-lookup"><span data-stu-id="17557-179">R is hello ratio of hello number of points that are inside hello circle toohello total number of points that are within hello square.</span></span> <span data-ttu-id="17557-180">hello större hello prov punkter används hello bättre hello uppskattning är.</span><span class="sxs-lookup"><span data-stu-id="17557-180">hello larger hello sample of points used, hello better hello estimate is.</span></span>

<span data-ttu-id="17557-181">Använd följande kommando toorun hello det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="17557-181">Use hello following command toorun this sample.</span></span> <span data-ttu-id="17557-182">Detta kommando använder 16 maps med 10 000 000 prover tooestimate hello värdet för pi:</span><span class="sxs-lookup"><span data-stu-id="17557-182">This command uses 16 maps with 10,000,000 samples each tooestimate hello value of pi:</span></span>

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

<span data-ttu-id="17557-183">hello värdet som returneras av kommandot är liknande för**3.14159155000000000000**.</span><span class="sxs-lookup"><span data-stu-id="17557-183">hello value returned by this command is similar too**3.14159155000000000000**.</span></span> <span data-ttu-id="17557-184">Referenser är hello första 10 decimaler pi 3.1415926535.</span><span class="sxs-lookup"><span data-stu-id="17557-184">For references, hello first 10 decimal places of pi are 3.1415926535.</span></span>

## <a name="10-gb-greysort-example"></a><span data-ttu-id="17557-185">10 GB Greysort exempel</span><span class="sxs-lookup"><span data-stu-id="17557-185">10 GB Greysort example</span></span>

<span data-ttu-id="17557-186">GraySort är en benchmark-sortering.</span><span class="sxs-lookup"><span data-stu-id="17557-186">GraySort is a benchmark sort.</span></span> <span data-ttu-id="17557-187">hello mått är hello sortera hastighet (TB per minut) som uppnås vid sortering av stora mängder data, vanligtvis en 100 TB minsta.</span><span class="sxs-lookup"><span data-stu-id="17557-187">hello metric is hello sort rate (TB/minute) that is achieved while sorting large amounts of data, usually a 100 TB minimum.</span></span>

<span data-ttu-id="17557-188">Det här exemplet använder en liten 10 GB data så att den kan köras relativt snabbt.</span><span class="sxs-lookup"><span data-stu-id="17557-188">This sample uses a modest 10 GB of data so that it can be run relatively quickly.</span></span> <span data-ttu-id="17557-189">Hej MapReduce program som utvecklats av Owen O'Malley och Arun Murthy används.</span><span class="sxs-lookup"><span data-stu-id="17557-189">It uses hello MapReduce applications developed by Owen O'Malley and Arun Murthy.</span></span> <span data-ttu-id="17557-190">Dessa program vann hello årliga allmänna (”daytona”) terabyte sortera prestandamått i 2009 med en andel 0.578 TB per minut (100 TB 173 minuter).</span><span class="sxs-lookup"><span data-stu-id="17557-190">These applications won hello annual general-purpose ("daytona") terabyte sort benchmark in 2009, with a rate of 0.578 TB/min (100 TB in 173 minutes).</span></span> <span data-ttu-id="17557-191">Mer information om den här och andra sorterings prestandamått finns hello [Sortbenchmark](http://sortbenchmark.org/) plats.</span><span class="sxs-lookup"><span data-stu-id="17557-191">For more information on this and other sorting benchmarks, see hello [Sortbenchmark](http://sortbenchmark.org/) site.</span></span>

<span data-ttu-id="17557-192">Det här exemplet använder tre olika MapReduce-program:</span><span class="sxs-lookup"><span data-stu-id="17557-192">This sample uses three sets of MapReduce programs:</span></span>

* <span data-ttu-id="17557-193">**TeraGen**: A MapReduce-program som skapar rader med data toosort</span><span class="sxs-lookup"><span data-stu-id="17557-193">**TeraGen**: A MapReduce program that generates rows of data toosort</span></span>

* <span data-ttu-id="17557-194">**TeraSort**: exempel hello indata och använder MapReduce toosort hello data i en total order</span><span class="sxs-lookup"><span data-stu-id="17557-194">**TeraSort**: Samples hello input data and uses MapReduce toosort hello data into a total order</span></span>

    <span data-ttu-id="17557-195">TeraSort är en standard MapReduce-sortering, förutom en anpassad partitioneraren.</span><span class="sxs-lookup"><span data-stu-id="17557-195">TeraSort is a standard MapReduce sort, except for a custom partitioner.</span></span> <span data-ttu-id="17557-196">Hej partitioneraren använder en sorterad lista över N-1 provtagning nycklar som definierar hello viktiga intervallet för varje minska.</span><span class="sxs-lookup"><span data-stu-id="17557-196">hello partitioner uses a sorted list of N-1 sampled keys that define hello key range for each reduce.</span></span> <span data-ttu-id="17557-197">I synnerhet alla nycklar sådana som exempel [i-1] < = nyckel < exempel [i] skickas tooreduce jag.</span><span class="sxs-lookup"><span data-stu-id="17557-197">In particular, all keys such that sample[i-1] <= key < sample[i] are sent tooreduce i.</span></span> <span data-ttu-id="17557-198">Den här partitioneraren garanterar att hello utdata för minskar i är mindre än hello utdata från minska i + 1.</span><span class="sxs-lookup"><span data-stu-id="17557-198">This partitioner guarantees that hello outputs of reduce i are all less than hello output of reduce i+1.</span></span>

* <span data-ttu-id="17557-199">**TeraValidate**: A MapReduce-program som verifierar att hello utdata sorteras globalt</span><span class="sxs-lookup"><span data-stu-id="17557-199">**TeraValidate**: A MapReduce program that validates that hello output is globally sorted</span></span>

    <span data-ttu-id="17557-200">Det skapar en mappning per fil i hello utdatakatalogen och varje mappning säkerställer att varje nyckel är mindre än eller lika toohello föregående.</span><span class="sxs-lookup"><span data-stu-id="17557-200">It creates one map per file in hello output directory, and each map ensures that each key is less than or equal toohello previous one.</span></span> <span data-ttu-id="17557-201">hello kartan funktionen genererar poster av hello först och senaste nycklarna för varje fil.</span><span class="sxs-lookup"><span data-stu-id="17557-201">hello map function generates records of hello first and last keys of each file.</span></span> <span data-ttu-id="17557-202">hello reducera garanterar att hello första nyckeln för filen i är större än hello senaste nyckel i filen i-1.</span><span class="sxs-lookup"><span data-stu-id="17557-202">hello reduce function ensures that hello first key of file i is greater than hello last key of file i-1.</span></span> <span data-ttu-id="17557-203">Eventuella problem rapporteras som utdata för hello minska fasen med hello nycklar som är i fel ordning.</span><span class="sxs-lookup"><span data-stu-id="17557-203">Any problems are reported as an output of hello reduce phase, with hello keys that are out of order.</span></span>

<span data-ttu-id="17557-204">Använd hello följande steg toogenerate data, sortera och sedan Validera hello utdata:</span><span class="sxs-lookup"><span data-stu-id="17557-204">Use hello following steps toogenerate data, sort, and then validate hello output:</span></span>

1. <span data-ttu-id="17557-205">Generera en 10 GB data som är lagrade toohello HDInsight klustret standardlagring på `/example/data/10GB-sort-input`:</span><span class="sxs-lookup"><span data-stu-id="17557-205">Generate 10 GB of data, which is stored toohello HDInsight cluster's default storage at `/example/data/10GB-sort-input`:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    <span data-ttu-id="17557-206">Hej `-Dmapred.map.tasks` talar om Hadoop hur många kartan uppgifter toouse för det här jobbet.</span><span class="sxs-lookup"><span data-stu-id="17557-206">hello `-Dmapred.map.tasks` tells Hadoop how many map tasks toouse for this job.</span></span> <span data-ttu-id="17557-207">hello sista två parametrar instruera hello jobbet toocreate 10 GB data och toostore den på `/example/data/10GB-sort-input`.</span><span class="sxs-lookup"><span data-stu-id="17557-207">hello final two parameters instruct hello job toocreate 10 GB of data and toostore it at `/example/data/10GB-sort-input`.</span></span>

2. <span data-ttu-id="17557-208">Använd följande kommando toosort hello data hello:</span><span class="sxs-lookup"><span data-stu-id="17557-208">Use hello following command toosort hello data:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    <span data-ttu-id="17557-209">Hej `-Dmapred.reduce.tasks` talar om Hadoop hur många minska uppgifter toouse för hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="17557-209">hello `-Dmapred.reduce.tasks` tells Hadoop how many reduce tasks toouse for hello job.</span></span> <span data-ttu-id="17557-210">hello sista två parametrar bara hello indata och utdata platser för data.</span><span class="sxs-lookup"><span data-stu-id="17557-210">hello final two parameters are just hello input and output locations for data.</span></span>

3. <span data-ttu-id="17557-211">Använd följande toovalidate hello data som genereras av hello sortera hello:</span><span class="sxs-lookup"><span data-stu-id="17557-211">Use hello following toovalidate hello data generated by hello sort:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a><span data-ttu-id="17557-212">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="17557-212">Next steps</span></span>

<span data-ttu-id="17557-213">Från den här artikeln får du lära dig hur toorun hello exempel medföljer hello Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="17557-213">From this article, you learned how toorun hello samples included with hello Linux-based HDInsight clusters.</span></span> <span data-ttu-id="17557-214">Självstudier om hur du använder Pig, finns Hive och MapReduce med HDInsight i hello följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="17557-214">For tutorials about using Pig, Hive, and MapReduce with HDInsight, see hello following topics:</span></span>

* <span data-ttu-id="17557-215">[Använda Pig med Hadoop i HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="17557-215">[Use Pig with Hadoop on HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="17557-216">[Använda Hive med Hadoop i HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="17557-216">[Use Hive with Hadoop on HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="17557-217">[Använda MapReduce med Hadoop i HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="17557-217">[Use MapReduce with Hadoop on HDInsight][hdinsight-use-mapreduce]</span></span>

[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
