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
# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>Använda MapReduce i Hadoop i HDInsight

Lär dig hur toorun MapReduce-jobb i HDInsight-kluster. Använd följande tabell toodiscover hello olika sätt att du kan använda MapReduce med HDInsight hello:

| **Använd den här**... | **.. .toodo detta** | ... med detta **klustret operativsystem** | .. .from detta **klientoperativsystem** |
|:--- |:--- |:--- |:--- |
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md) |Kommandot hello Hadoop via **SSH** |Linux |Linux, Unix, Mac OS X eller Windows |
| [REST](hdinsight-hadoop-use-mapreduce-curl.md) |Skickar hello jobb via fjärranslutning med hjälp av **REST** (exemplen använder cURL) |Linux- eller Windows |Linux, Unix, Mac OS X eller Windows |
| [Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) |Skickar hello jobb via fjärranslutning med hjälp av **Windows PowerShell** |Linux- eller Windows |Windows |
| [Fjärrskrivbord](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 och 3.3) |Kommandot hello Hadoop via **fjärrskrivbord** |Windows |Windows |

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="whatis"></a>Vad är MapReduce

Hadoop MapReduce är ett ramverk för programvara för att skriva jobb som bearbetar stora mängder data. Indata är uppdelad i oberoende segment som sedan bearbetas parallellt över hello noder i klustret. Ett MapReduce-jobb består av två funktioner:

* **Mapper**: förbrukar indata, analyserar (vanligtvis med filter och sortering operations) och skickar tupplar (nyckel / värde-par)

* **Reducer**: förbrukar tupplar som sänds av hello Mapper och utför en sammanfattande som skapar ett mindre, kombinerade resultat från hello Mapper data

Ett grundläggande word antal MapReduce-jobbet exempel illustreras i följande diagram hello:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

hello utdata för jobbet är en beräkning av hur många gånger varje ord uppstod i hello text som har analyserats.

* hello mapper tar raderna från hello-indata som indata och delar i ord. Den genererar en nyckel/värde-par varje gång ett ord inträffar hello Word följs av en 1. hello-utdata är sorterad innan den skickas tooreducer.
* Hej reducer summerar dessa enskilda antalet för varje ord och genererar ett enda nyckel/värde-par som innehåller hello ordet följt av hello summan av dess förekomster.

MapReduce kan implementeras på olika språk. Java är de vanligaste hello-implementering och används i exempelsyfte i det här dokumentet.

## <a name="development-languages"></a>Programmeringsspråk

Språk eller ramverk som baseras på Java och hello Java Virtual Machine kan köras direkt som ett MapReduce-jobb. hello-exempel som används i det här dokumentet är ett MapReduce för Java-program. Icke-Java-språk, till exempel C#, Python eller fristående körbara filer, måste använda Hadoop-strömning.

Hadoop streaming kommunicerar med hello mapper och reducer via STDIN och STDOUT. hello mapper reducer läsa data till en rad i taget från STDIN och skriva hello utdata tooSTDOUT. Varje rad läsa eller sänds av hello mapper och reducer måste vara i hello-format för nyckel/värde-par avgränsas med ett tabbtecken:

    [key]/t[value]

Mer information finns i [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html).

Exempel på användning av Hadoop-strömning med HDInsight finns i följande dokument hello:

* [Utveckla C# MapReduce-jobb](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [Utveckla Python MapReduce-jobb](hdinsight-hadoop-streaming-python.md)

## <a id="data"></a>Exempeldata

HDInsight tillhandahåller olika exempel datauppsättningar som lagras i hello `/example/data` och `/HdiSamples` directory. Dessa kataloger finns i hello standardlagring för klustret. I det här dokumentet använder vi hello `/example/data/gutenberg/davinci.txt` fil. Den här filen innehåller hello anteckningsböcker av Leonardo Da Vinci.

## <a id="job"></a>Exempel MapReduce

Ett exempel MapReduce word antal program ingår i ditt HDInsight-kluster. Det här exemplet finns på `/example/jars/hadoop-mapreduce-examples.jar` på hello standardlagring för klustret.

hello följande Java-kod är hello hello MapReduce program som ingår i hello `hadoop-mapreduce-examples.jar` fil:

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

Egna MapReduce-program finns i anvisningarna toowrite hello följande dokument:

* [Utveckla Java-MapReduce-program för HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Utveckla Python MapReduce program för HDInsight](hdinsight-hadoop-streaming-python.md)

## <a id="run"></a>Kör hello MapReduce

HDInsight kan köra HiveQL jobb med hjälp av olika metoder. Använd hello efter tabellen toodecide vilken metod som passar dig sedan länken hello en genomgång.

| **Använd den här**... | **.. .toodo detta** | ... med detta **klustret operativsystem** | .. .from detta **klientoperativsystem** |
|:--- |:--- |:--- |:--- |
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md) |Kommandot hello Hadoop via **SSH** |Linux |Linux, Unix, Mac OS X eller Windows |
| [CURL](hdinsight-hadoop-use-mapreduce-curl.md) |Skickar hello jobb via fjärranslutning med hjälp av **REST** |Linux- eller Windows |Linux, Unix, Mac OS X eller Windows |
| [Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) |Skickar hello jobb via fjärranslutning med hjälp av **Windows PowerShell** |Linux- eller Windows |Windows |
| [Fjärrskrivbord](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 och 3.3) |Kommandot hello Hadoop via **fjärrskrivbord** |Windows |Windows |

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="nextsteps"></a>Nästa steg

toolearn mer information om hur du arbetar med data i HDInsight, se hello följande dokument:

* [Utveckla Java-MapReduce-program för HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Utveckla Python strömning MapReduce program för HDInsight](hdinsight-hadoop-streaming-python.md)

* [Utveckla skållning MapReduce-jobb med Apache Hadoop i HDInsight](hdinsight-hadoop-mapreduce-scalding.md)

* [Använda Hive med HDInsight][hdinsight-use-hive]

* [Använda Pig med HDInsight][hdinsight-use-pig]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
