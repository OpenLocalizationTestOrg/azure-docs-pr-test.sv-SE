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
# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>Använda MapReduce med Hadoop i HDInsight med SSH

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Lär dig hur toosubmit MapReduce-jobb från en tooHDInsight för anslutning av SSH (Secure Shell).

> [!NOTE]
> Om du redan är bekant med Linux-baserade Hadoop-servrar, men du är ny tooHDInsight, se [Linux-baserat HDInsight tips](hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Förhandskrav

* Ett kluster på Linux-baserat HDInsight (Hadoop på HDInsight)

  > [!IMPORTANT]
  > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* En SSH-klient. Mer information finns i [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a id="ssh"></a>Ansluta med SSH

Ansluta toohello kluster med SSH. Till exempel följande kommando hello ansluter tooa kluster med namnet **myhdinsight**:

```bash
ssh admin@myhdinsight-ssh.azurehdinsight.net
```

**Om du använder en nyckel för certifikat för SSH-autentisering**, kanske du måste toospecify hello platsen för hello privat nyckel i klientsystemet, till exempel:

```bash
ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net
```

**Om du använder ett lösenord för SSH-autentisering**, behöver du tooprovide hello lösenord när du uppmanas.

Mer information om hur du använder SSH med HDInsight finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="hadoop"></a>Använda Hadoop-kommandon

1. När du är ansluten toohello HDInsight-kluster, kommando Använd hello följande toostart ett MapReduce-jobb:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    Detta kommando startar hello `wordcount` -klassen, som ingår i hello `hadoop-mapreduce-examples.jar` fil. Den använder hello `/example/data/gutenberg/davinci.txt` dokumentet som indata och utdata lagras på `/example/data/WordCountOutput`.

    > [!NOTE]
    > Mer information om den här MapReduce-jobb och hello exempeldata finns [använda MapReduce i Hadoop på HDInsight](hdinsight-use-mapreduce.md).

2. hello jobbet sänder ut information när den bearbetar och returnerar information liknande toohello efter text när hello jobbet har slutförts:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. När hello jobbet har slutförts, kan du använda hello följande kommando toolist hello utgående filer:

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    Det här kommandot visar två filer, `_SUCCESS` och `part-r-00000`. Hej `part-r-00000` filen innehåller hello utdata för jobbet.

    > [!NOTE]
    > Vissa MapReduce-jobb kan delas upp hello resultat över flera **del-r-###** filer. I så fall använder hello ### suffix tooindicate hello ordning hello-filer.

4. tooview hello utdata, Använd hello följande kommando:

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    Detta kommando visar en lista över hello ord som ingår i hello **wasb://example/data/gutenberg/davinci.txt** fil- och hello antal varje ord inträffade. hello är följande ett exempel på hello data som finns i filen hello:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Sammanfattning

Som du kan se ger Hadoop kommandon ett enkelt sätt toorun MapReduce-jobb i ett HDInsight-kluster och sedan visa hello jobbutdata.

## <a id="nextsteps"></a>Nästa steg

Allmän information om MapReduce-jobb i HDInsight:

* [Använda MapReduce på Hadoop och HDInsight](hdinsight-use-mapreduce.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)
* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)
