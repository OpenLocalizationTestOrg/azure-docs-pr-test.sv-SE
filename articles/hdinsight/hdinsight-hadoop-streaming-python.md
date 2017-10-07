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
# <a name="develop-python-streaming-mapreduce-programs-for-hdinsight"></a>Utveckla Python strömning MapReduce program för HDInsight

Lär dig hur toouse Python med strömmande MapReduce-åtgärder. Hadoop innehåller ett strömmande API för MapReduce som aktiverar toowrite kartan och minska funktioner på andra språk än Java. hello stegen i det här dokumentet implementera hello kartan och minska komponenter i Python.

## <a name="prerequisites"></a>Krav

* En Linux-baserade Hadoop på HDInsight-kluster

  > [!IMPORTANT]
  > hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* En textredigerare

  > [!IMPORTANT]
  > hello textredigerare måste använda LF hello rad avslutas. Med hjälp av en rad avslutades av CRLF orsakar fel när du kör hello MapReduce-jobb på Linux-baserade HDInsight-kluster.

* Hej `ssh` och `scp` kommandon, eller [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)

## <a name="word-count"></a>Räkna ord

Det här exemplet är en grundläggande ordräkning genomföras i en python en mapper och reducer. hello mapper bryter meningar i individuella ord och hello reducer aggregerar hello ord och räknar tooproduce hello utdata.

hello följande flödesschema visar vad som händer under hello kartan och minska faser.

![Bild av hello mapreduce-processen](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="streaming-mapreduce"></a>Strömmande MapReduce

Hadoop kan toospecify en fil som innehåller hello kartan och minska logik som används av ett jobb. hello särskilda krav för hello mappa och minska logik är:

* **Inkommande**: hello kartan och minska komponenter måste läsa indata från STDIN.
* **Utdata**: hello kartan och minska komponenter måste skriva utdata data tooSTDOUT.
* **Dataformatet**: hello data används och producerade måste vara ett nyckel/värde-par, avgränsade med semikolon fliken.

Python kan enkelt hantera dessa krav med hjälp av hello `sys` modulen tooread från STDIN och använda `print` tooprint tooSTDOUT. hello återstående aktivitet helt enkelt formatera hello data med en flik (`\t`) tecken mellan hello nyckel och värde.

## <a name="create-hello-mapper-and-reducer"></a>Skapa hello mapper och reducer

1. Skapa en fil med namnet `mapper.py` och Använd hello efter koden som hello innehåll:

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

2. Skapa en fil med namnet **reducer.py** och Använd hello efter koden som hello innehåll:

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

## <a name="run-using-powershell"></a>Kör med PowerShell

tooensure att filerna har hello rätt radbrytningar, Använd hello följande PowerShell-skript:

[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]

Använd följande PowerShell-skriptet tooupload hello filer hello, kör hello jobb och visa hello utdata:

[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]

## <a name="run-from-an-ssh-session"></a>Kör från en SSH-session

1. Från din utvecklingsmiljö i hello samma katalog som `mapper.py` och `reducer.py` filer, använda hello följande kommando:

    ```bash
    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:
    ```

    Ersätt `username` med hello SSH-användarnamn för klustret, och `clustername` med hello namnet på klustret.

    Det här kommandot kopierar hello filer från lokala system hello toohello huvudnod.

    > [!NOTE]
    > Om du har använt ett lösenord toosecure SSH-konto kan ombeds du hello lösenord. Om du använder en SSH-nyckel, kanske toouse hello `-i` parameter och hello sökvägen toohello den privata nyckeln. Till exempel `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

2. Anslut toohello kluster med hjälp av SSH:

    ```bash
    ssh username@clustername-ssh.azurehdinsight.net`
    ```

    Mer information om finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

3. tooensure hello mapper.py och reducer.py har hello korrigera radbrytningar genom att använda hello följande kommandon:

    ```bash
    perl -pi -e 's/\r\n/\n/g' mapper.py
    perl -pi -e 's/\r\n/\n/g' reducer.py
    ```

4. Använd hello efter kommandot toostart hello MapReduce-jobb.

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
    ```

    Det här kommandot har hello följande delar:

   * **hadoop-streaming.jar**: används när du utför strömmande MapReduce-åtgärder. Det gränssnitt Hadoop med hello externa MapReduce kod som du anger.

   * **-filer**: lägger till hello angivna filer toohello MapReduce-jobb.

   * **-mapper**: talar om för Hadoop som filen toouse som hello mapper.

   * **-reducer**: talar om för Hadoop som filen toouse som hello reducer.

   * **-inkommande**: hello indatafil som vi ska räkna ord från.

   * **-utdata**: hello-katalog som hello utdata skrivs till.

    Eftersom hello MapReduce-jobb fungerar, visas hello process som procenttal.

        05-02-15 19:01:04 INFO mapreduce. Jobbet: karta 0% minska 0% 05-02-15 19:01:16 INFO mapreduce. Jobbet: karta 100% minska 0% 05-02-15 19:01:27 INFO mapreduce. Jobbet: karta 100% minska 100%


5. tooview hello utdata, Använd hello följande kommando:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    Det här kommandot visar en lista över ord och hur många gånger hello word inträffade.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur toouse strömning MapRedcue jobb med HDInsight, använder du följande länkar tooexplore hello andra sätt toowork med Azure HDInsight.

* [Använda Hive med HDInsight](hdinsight-use-hive.md)
* [Använda Pig med HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce-jobb med HDInsight](hdinsight-use-mapreduce.md)
