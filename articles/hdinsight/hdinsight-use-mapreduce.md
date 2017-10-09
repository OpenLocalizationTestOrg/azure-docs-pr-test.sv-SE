---
title: aaaMapReduce med Hadoop i HDInsight | Microsoft Docs
description: "Lär dig hur toorun MapReduce-jobb på Hadoop i HDInsight-kluster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7f321501-d62c-4ffc-b5d6-102ecba6dd76
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 0cf7ad0e6769e678be64f9e4ec8ed7a214ab7af2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a><span data-ttu-id="f05ae-103">Använda MapReduce i Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="f05ae-103">Use MapReduce in Hadoop on HDInsight</span></span>

<span data-ttu-id="f05ae-104">Lär dig hur toorun MapReduce-jobb i HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f05ae-104">Learn how toorun MapReduce jobs on HDInsight clusters.</span></span> <span data-ttu-id="f05ae-105">Använd följande tabell toodiscover hello olika sätt att du kan använda MapReduce med HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="f05ae-105">Use hello following table toodiscover hello various ways that MapReduce can be used with HDInsight:</span></span>

| <span data-ttu-id="f05ae-106">**Använd den här**...</span><span class="sxs-lookup"><span data-stu-id="f05ae-106">**Use this**...</span></span> | <span data-ttu-id="f05ae-107">**.. .toodo detta**</span><span class="sxs-lookup"><span data-stu-id="f05ae-107">**...toodo this**</span></span> | <span data-ttu-id="f05ae-108">... med detta **klustret operativsystem**</span><span class="sxs-lookup"><span data-stu-id="f05ae-108">...with this **cluster operating system**</span></span> | <span data-ttu-id="f05ae-109">.. .from detta **klientoperativsystem**</span><span class="sxs-lookup"><span data-stu-id="f05ae-109">...from this **client operating system**</span></span> |
|:--- |:--- |:--- |:--- |
| [<span data-ttu-id="f05ae-110">SSH</span><span class="sxs-lookup"><span data-stu-id="f05ae-110">SSH</span></span>](hdinsight-hadoop-use-mapreduce-ssh.md) |<span data-ttu-id="f05ae-111">Kommandot hello Hadoop via **SSH**</span><span class="sxs-lookup"><span data-stu-id="f05ae-111">Use hello Hadoop command through **SSH**</span></span> |<span data-ttu-id="f05ae-112">Linux</span><span class="sxs-lookup"><span data-stu-id="f05ae-112">Linux</span></span> |<span data-ttu-id="f05ae-113">Linux, Unix, Mac OS X eller Windows</span><span class="sxs-lookup"><span data-stu-id="f05ae-113">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="f05ae-114">REST</span><span class="sxs-lookup"><span data-stu-id="f05ae-114">REST</span></span>](hdinsight-hadoop-use-mapreduce-curl.md) |<span data-ttu-id="f05ae-115">Skickar hello jobb via fjärranslutning med hjälp av **REST** (exemplen använder cURL)</span><span class="sxs-lookup"><span data-stu-id="f05ae-115">Submit hello job remotely by using **REST** (examples use cURL)</span></span> |<span data-ttu-id="f05ae-116">Linux- eller Windows</span><span class="sxs-lookup"><span data-stu-id="f05ae-116">Linux or Windows</span></span> |<span data-ttu-id="f05ae-117">Linux, Unix, Mac OS X eller Windows</span><span class="sxs-lookup"><span data-stu-id="f05ae-117">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="f05ae-118">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="f05ae-118">Windows PowerShell</span></span>](hdinsight-hadoop-use-mapreduce-powershell.md) |<span data-ttu-id="f05ae-119">Skickar hello jobb via fjärranslutning med hjälp av **Windows PowerShell**</span><span class="sxs-lookup"><span data-stu-id="f05ae-119">Submit hello job remotely by using **Windows PowerShell**</span></span> |<span data-ttu-id="f05ae-120">Linux- eller Windows</span><span class="sxs-lookup"><span data-stu-id="f05ae-120">Linux or Windows</span></span> |<span data-ttu-id="f05ae-121">Windows</span><span class="sxs-lookup"><span data-stu-id="f05ae-121">Windows</span></span> |
| <span data-ttu-id="f05ae-122">[Fjärrskrivbord](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 och 3.3)</span><span class="sxs-lookup"><span data-stu-id="f05ae-122">[Remote Desktop](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="f05ae-123">Kommandot hello Hadoop via **fjärrskrivbord**</span><span class="sxs-lookup"><span data-stu-id="f05ae-123">Use hello Hadoop command through **Remote Desktop**</span></span> |<span data-ttu-id="f05ae-124">Windows</span><span class="sxs-lookup"><span data-stu-id="f05ae-124">Windows</span></span> |<span data-ttu-id="f05ae-125">Windows</span><span class="sxs-lookup"><span data-stu-id="f05ae-125">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="f05ae-126">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f05ae-126">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f05ae-127">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f05ae-127">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="f05ae-128"><a id="whatis"></a>Vad är MapReduce</span><span class="sxs-lookup"><span data-stu-id="f05ae-128"><a id="whatis"></a>What is MapReduce</span></span>

<span data-ttu-id="f05ae-129">Hadoop MapReduce är ett ramverk för programvara för att skriva jobb som bearbetar stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="f05ae-129">Hadoop MapReduce is a software framework for writing jobs that process vast amounts of data.</span></span> <span data-ttu-id="f05ae-130">Indata är uppdelad i oberoende segment som sedan bearbetas parallellt över hello noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="f05ae-130">Input data is split into independent chunks, which are then processed in parallel across hello nodes in your cluster.</span></span> <span data-ttu-id="f05ae-131">Ett MapReduce-jobb består av två funktioner:</span><span class="sxs-lookup"><span data-stu-id="f05ae-131">A MapReduce job consists of two functions:</span></span>

* <span data-ttu-id="f05ae-132">**Mapper**: förbrukar indata, analyserar (vanligtvis med filter och sortering operations) och skickar tupplar (nyckel / värde-par)</span><span class="sxs-lookup"><span data-stu-id="f05ae-132">**Mapper**: Consumes input data, analyzes it (usually with filter and sorting operations), and emits tuples (key-value pairs)</span></span>

* <span data-ttu-id="f05ae-133">**Reducer**: förbrukar tupplar som sänds av hello Mapper och utför en sammanfattande som skapar ett mindre, kombinerade resultat från hello Mapper data</span><span class="sxs-lookup"><span data-stu-id="f05ae-133">**Reducer**: Consumes tuples emitted by hello Mapper and performs a summary operation that creates a smaller, combined result from hello Mapper data</span></span>

<span data-ttu-id="f05ae-134">Ett grundläggande word antal MapReduce-jobbet exempel illustreras i följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="f05ae-134">A basic word count MapReduce job example is illustrated in hello following diagram:</span></span>

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

<span data-ttu-id="f05ae-136">hello utdata för jobbet är en beräkning av hur många gånger varje ord uppstod i hello text som har analyserats.</span><span class="sxs-lookup"><span data-stu-id="f05ae-136">hello output of this job is a count of how many times each word occurred in hello text that was analyzed.</span></span>

* <span data-ttu-id="f05ae-137">hello mapper tar raderna från hello-indata som indata och delar i ord.</span><span class="sxs-lookup"><span data-stu-id="f05ae-137">hello mapper takes each line from hello input text as an input and breaks it into words.</span></span> <span data-ttu-id="f05ae-138">Den genererar en nyckel/värde-par varje gång ett ord inträffar hello Word följs av en 1.</span><span class="sxs-lookup"><span data-stu-id="f05ae-138">It emits a key/value pair each time a word occurs of hello word is followed by a 1.</span></span> <span data-ttu-id="f05ae-139">hello-utdata är sorterad innan den skickas tooreducer.</span><span class="sxs-lookup"><span data-stu-id="f05ae-139">hello output is sorted before sending it tooreducer.</span></span>
* <span data-ttu-id="f05ae-140">Hej reducer summerar dessa enskilda antalet för varje ord och genererar ett enda nyckel/värde-par som innehåller hello ordet följt av hello summan av dess förekomster.</span><span class="sxs-lookup"><span data-stu-id="f05ae-140">hello reducer sums these individual counts for each word and emits a single key/value pair that contains hello word followed by hello sum of its occurrences.</span></span>

<span data-ttu-id="f05ae-141">MapReduce kan implementeras på olika språk.</span><span class="sxs-lookup"><span data-stu-id="f05ae-141">MapReduce can be implemented in various languages.</span></span> <span data-ttu-id="f05ae-142">Java är de vanligaste hello-implementering och används i exempelsyfte i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f05ae-142">Java is hello most common implementation, and is used for demonstration purposes in this document.</span></span>

## <a name="development-languages"></a><span data-ttu-id="f05ae-143">Programmeringsspråk</span><span class="sxs-lookup"><span data-stu-id="f05ae-143">Development languages</span></span>

<span data-ttu-id="f05ae-144">Språk eller ramverk som baseras på Java och hello Java Virtual Machine kan köras direkt som ett MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="f05ae-144">Languages or frameworks that are based on Java and hello Java Virtual Machine can be ran directly as a MapReduce job.</span></span> <span data-ttu-id="f05ae-145">hello-exempel som används i det här dokumentet är ett MapReduce för Java-program.</span><span class="sxs-lookup"><span data-stu-id="f05ae-145">hello example used in this document is a Java MapReduce application.</span></span> <span data-ttu-id="f05ae-146">Icke-Java-språk, till exempel C#, Python eller fristående körbara filer, måste använda Hadoop-strömning.</span><span class="sxs-lookup"><span data-stu-id="f05ae-146">Non-Java languages, such as C#, Python, or standalone executables, must use Hadoop streaming.</span></span>

<span data-ttu-id="f05ae-147">Hadoop streaming kommunicerar med hello mapper och reducer via STDIN och STDOUT.</span><span class="sxs-lookup"><span data-stu-id="f05ae-147">Hadoop streaming communicates with hello mapper and reducer over STDIN and STDOUT.</span></span> <span data-ttu-id="f05ae-148">hello mapper reducer läsa data till en rad i taget från STDIN och skriva hello utdata tooSTDOUT.</span><span class="sxs-lookup"><span data-stu-id="f05ae-148">hello mapper and reducer read data a line at a time from STDIN, and write hello output tooSTDOUT.</span></span> <span data-ttu-id="f05ae-149">Varje rad läsa eller sänds av hello mapper och reducer måste vara i hello-format för nyckel/värde-par avgränsas med ett tabbtecken:</span><span class="sxs-lookup"><span data-stu-id="f05ae-149">Each line read or emitted by hello mapper and reducer must be in hello format of a key/value pair, delimited by a tab character:</span></span>

    [key]/t[value]

<span data-ttu-id="f05ae-150">Mer information finns i [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html).</span><span class="sxs-lookup"><span data-stu-id="f05ae-150">For more information, see [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html).</span></span>

<span data-ttu-id="f05ae-151">Exempel på användning av Hadoop-strömning med HDInsight finns i följande dokument hello:</span><span class="sxs-lookup"><span data-stu-id="f05ae-151">For examples of using Hadoop streaming with HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="f05ae-152">Utveckla C# MapReduce-jobb</span><span class="sxs-lookup"><span data-stu-id="f05ae-152">Develop C# MapReduce jobs</span></span>](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [<span data-ttu-id="f05ae-153">Utveckla Python MapReduce-jobb</span><span class="sxs-lookup"><span data-stu-id="f05ae-153">Develop Python MapReduce jobs</span></span>](hdinsight-hadoop-streaming-python.md)

## <span data-ttu-id="f05ae-154"><a id="data"></a>Exempeldata</span><span class="sxs-lookup"><span data-stu-id="f05ae-154"><a id="data"></a>Example data</span></span>

<span data-ttu-id="f05ae-155">HDInsight tillhandahåller olika exempel datauppsättningar som lagras i hello `/example/data` och `/HdiSamples` directory.</span><span class="sxs-lookup"><span data-stu-id="f05ae-155">HDInsight provides various example data sets, which are stored in hello `/example/data` and `/HdiSamples` directory.</span></span> <span data-ttu-id="f05ae-156">Dessa kataloger finns i hello standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="f05ae-156">These directories are in hello default storage for your cluster.</span></span> <span data-ttu-id="f05ae-157">I det här dokumentet använder vi hello `/example/data/gutenberg/davinci.txt` fil.</span><span class="sxs-lookup"><span data-stu-id="f05ae-157">In this document, we use hello `/example/data/gutenberg/davinci.txt` file.</span></span> <span data-ttu-id="f05ae-158">Den här filen innehåller hello anteckningsböcker av Leonardo Da Vinci.</span><span class="sxs-lookup"><span data-stu-id="f05ae-158">This file contains hello notebooks of Leonardo Da Vinci.</span></span>

## <span data-ttu-id="f05ae-159"><a id="job"></a>Exempel MapReduce</span><span class="sxs-lookup"><span data-stu-id="f05ae-159"><a id="job"></a>Example MapReduce</span></span>

<span data-ttu-id="f05ae-160">Ett exempel MapReduce word antal program ingår i ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f05ae-160">An example MapReduce word count application is included with your HDInsight cluster.</span></span> <span data-ttu-id="f05ae-161">Det här exemplet finns på `/example/jars/hadoop-mapreduce-examples.jar` på hello standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="f05ae-161">This example is located at `/example/jars/hadoop-mapreduce-examples.jar` on hello default storage for your cluster.</span></span>

<span data-ttu-id="f05ae-162">hello följande Java-kod är hello hello MapReduce program som ingår i hello `hadoop-mapreduce-examples.jar` fil:</span><span class="sxs-lookup"><span data-stu-id="f05ae-162">hello following Java code is hello source of hello MapReduce application contained in hello `hadoop-mapreduce-examples.jar` file:</span></span>

```java
package org.apache.hadoop.examples;

import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;

public class WordCount {

    public static class TokenizerMapper
        extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
        StringTokenizer itr = new StringTokenizer(value.toString());
        while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        }
    }
    }

    public static class IntSumReducer
        extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                        Context context
                        ) throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
        sum += val.get();
        }
        result.set(sum);
        context.write(key, result);
    }
    }

    public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length != 2) {
        System.err.println("Usage: wordcount <in> <out>");
        System.exit(2);
    }
    Job job = new Job(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

<span data-ttu-id="f05ae-163">Egna MapReduce-program finns i anvisningarna toowrite hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="f05ae-163">For instructions toowrite your own MapReduce applications, see hello following documents:</span></span>

* [<span data-ttu-id="f05ae-164">Utveckla Java-MapReduce-program för HDInsight</span><span class="sxs-lookup"><span data-stu-id="f05ae-164">Develop Java MapReduce applications for HDInsight</span></span>](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [<span data-ttu-id="f05ae-165">Utveckla Python MapReduce program för HDInsight</span><span class="sxs-lookup"><span data-stu-id="f05ae-165">Develop Python MapReduce applications for HDInsight</span></span>](hdinsight-hadoop-streaming-python.md)

## <span data-ttu-id="f05ae-166"><a id="run"></a>Kör hello MapReduce</span><span class="sxs-lookup"><span data-stu-id="f05ae-166"><a id="run"></a>Run hello MapReduce</span></span>

<span data-ttu-id="f05ae-167">HDInsight kan köra HiveQL jobb med hjälp av olika metoder.</span><span class="sxs-lookup"><span data-stu-id="f05ae-167">HDInsight can run HiveQL jobs by using various methods.</span></span> <span data-ttu-id="f05ae-168">Använd hello efter tabellen toodecide vilken metod som passar dig sedan länken hello en genomgång.</span><span class="sxs-lookup"><span data-stu-id="f05ae-168">Use hello following table toodecide which method is right for you, then follow hello link for a walkthrough.</span></span>

| <span data-ttu-id="f05ae-169">**Använd den här**...</span><span class="sxs-lookup"><span data-stu-id="f05ae-169">**Use this**...</span></span> | <span data-ttu-id="f05ae-170">**.. .toodo detta**</span><span class="sxs-lookup"><span data-stu-id="f05ae-170">**...toodo this**</span></span> | <span data-ttu-id="f05ae-171">... med detta **klustret operativsystem**</span><span class="sxs-lookup"><span data-stu-id="f05ae-171">...with this **cluster operating system**</span></span> | <span data-ttu-id="f05ae-172">.. .from detta **klientoperativsystem**</span><span class="sxs-lookup"><span data-stu-id="f05ae-172">...from this **client operating system**</span></span> |
|:--- |:--- |:--- |:--- |
| [<span data-ttu-id="f05ae-173">SSH</span><span class="sxs-lookup"><span data-stu-id="f05ae-173">SSH</span></span>](hdinsight-hadoop-use-mapreduce-ssh.md) |<span data-ttu-id="f05ae-174">Kommandot hello Hadoop via **SSH**</span><span class="sxs-lookup"><span data-stu-id="f05ae-174">Use hello Hadoop command through **SSH**</span></span> |<span data-ttu-id="f05ae-175">Linux</span><span class="sxs-lookup"><span data-stu-id="f05ae-175">Linux</span></span> |<span data-ttu-id="f05ae-176">Linux, Unix, Mac OS X eller Windows</span><span class="sxs-lookup"><span data-stu-id="f05ae-176">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="f05ae-177">CURL</span><span class="sxs-lookup"><span data-stu-id="f05ae-177">Curl</span></span>](hdinsight-hadoop-use-mapreduce-curl.md) |<span data-ttu-id="f05ae-178">Skickar hello jobb via fjärranslutning med hjälp av **REST**</span><span class="sxs-lookup"><span data-stu-id="f05ae-178">Submit hello job remotely by using **REST**</span></span> |<span data-ttu-id="f05ae-179">Linux- eller Windows</span><span class="sxs-lookup"><span data-stu-id="f05ae-179">Linux or Windows</span></span> |<span data-ttu-id="f05ae-180">Linux, Unix, Mac OS X eller Windows</span><span class="sxs-lookup"><span data-stu-id="f05ae-180">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="f05ae-181">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="f05ae-181">Windows PowerShell</span></span>](hdinsight-hadoop-use-mapreduce-powershell.md) |<span data-ttu-id="f05ae-182">Skickar hello jobb via fjärranslutning med hjälp av **Windows PowerShell**</span><span class="sxs-lookup"><span data-stu-id="f05ae-182">Submit hello job remotely by using **Windows PowerShell**</span></span> |<span data-ttu-id="f05ae-183">Linux- eller Windows</span><span class="sxs-lookup"><span data-stu-id="f05ae-183">Linux or Windows</span></span> |<span data-ttu-id="f05ae-184">Windows</span><span class="sxs-lookup"><span data-stu-id="f05ae-184">Windows</span></span> |
| <span data-ttu-id="f05ae-185">[Fjärrskrivbord](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 och 3.3)</span><span class="sxs-lookup"><span data-stu-id="f05ae-185">[Remote Desktop](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="f05ae-186">Kommandot hello Hadoop via **fjärrskrivbord**</span><span class="sxs-lookup"><span data-stu-id="f05ae-186">Use hello Hadoop command through **Remote Desktop**</span></span> |<span data-ttu-id="f05ae-187">Windows</span><span class="sxs-lookup"><span data-stu-id="f05ae-187">Windows</span></span> |<span data-ttu-id="f05ae-188">Windows</span><span class="sxs-lookup"><span data-stu-id="f05ae-188">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="f05ae-189">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f05ae-189">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f05ae-190">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f05ae-190">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="f05ae-191"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f05ae-191"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="f05ae-192">toolearn mer information om hur du arbetar med data i HDInsight, se hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="f05ae-192">toolearn more about working with data in HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="f05ae-193">Utveckla Java-MapReduce-program för HDInsight</span><span class="sxs-lookup"><span data-stu-id="f05ae-193">Develop Java MapReduce programs for HDInsight</span></span>](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [<span data-ttu-id="f05ae-194">Utveckla Python strömning MapReduce program för HDInsight</span><span class="sxs-lookup"><span data-stu-id="f05ae-194">Develop Python streaming MapReduce programs for HDInsight</span></span>](hdinsight-hadoop-streaming-python.md)

* [<span data-ttu-id="f05ae-195">Utveckla skållning MapReduce-jobb med Apache Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="f05ae-195">Develop Scalding MapReduce jobs with Apache Hadoop on HDInsight</span></span>](hdinsight-hadoop-mapreduce-scalding.md)

* <span data-ttu-id="f05ae-196">[Använda Hive med HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="f05ae-196">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>

* <span data-ttu-id="f05ae-197">[Använda Pig med HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="f05ae-197">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
