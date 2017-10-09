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
# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Använda MapReduce i Hadoop i HDInsight med fjärrskrivbord
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

I den här artikeln kommer du lär dig hur tooconnect tooa Hadoop på HDInsight-kluster med hjälp av fjärrskrivbord och kör sedan MapReduce-jobb med hello Hadoop-kommando.

> [!IMPORTANT]
> Fjärrskrivbord är bara tillgänglig på Windows-baserade HDInsight-kluster. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> För HDInsight 3.4 eller större finns [använda MapReduce med SSH](hdinsight-hadoop-use-mapreduce-ssh.md) för information om hur du ansluter toohello HDInsight-kluster och kör MapReduce-jobb.

## <a id="prereq"></a>Förhandskrav
toocomplete hello stegen i den här artikeln, behöver du hello följande:

* Ett kluster med Windows-baserade HDInsight (Hadoop på HDInsight)
* En klientdator som kör Windows 10, Windows 8 eller Windows 7

## <a id="connect"></a>Ansluta med fjärrskrivbord
Aktivera Fjärrskrivbord för hello HDInsight-kluster och sedan ansluta tooit genom att följa anvisningarna hello på [ansluta tooHDInsight kluster med RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hadoop"></a>Använd hello Hadoop-kommando
När du är ansluten toohello desktop för hello HDInsight-kluster, Använd hello följande steg toorun ett MapReduce-jobb med hello Hadoop-kommando:

1. Starta från hello HDInsight desktop hello **Hadoop kommandoraden**. Då öppnas en ny kommandotolk i hello **c:\apps\dist\hadoop-&lt;versionsnummer >** directory.

   > [!NOTE]
   > hello versionsnumret ändras när Hadoop uppdateras. Hej **HADOOP_HOME** miljövariabeln kan vara används toofind hello sökväg. Till exempel `cd %HADOOP_HOME%` ändringar kataloger toohello Hadoop katalog, utan att du tooknow hello-versionsnumret.
   >
   >
2. toouse hello **Hadoop** kommandot toorun ett exempel MapReduce-jobb, använda hello följande kommando:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Detta startar hello **wordcount** -klassen, som ingår i hello **hadoop-mapreduce-examples.jar** filen i hello aktuell katalog. Som indata, används hello **wasb://example/data/gutenberg/davinci.txt** dokument och utdata är lagrade på: **wasb: / / / exempel/data/WordCountOutput**.

   > [!NOTE]
   > Mer information om den här MapReduce-jobb och hello exempeldata finns <a href="hdinsight-use-mapreduce.md">använda MapReduce i HDInsight Hadoop</a>.
   >
   >
3. hello jobbet sänder ut information när den bearbetas och returnerar information liknande toohello följande när hello jobbet har slutförts:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. När hello jobbet har slutförts, kan du använda hello följande kommando toolist hello utgående filer lagras på **wasb://example/data/WordCountOutput**:

        hadoop fs -ls wasb:///example/data/WordCountOutput

    Visas två filer, **_SUCCESS** och **del-r-00000**. Hej **del-r-00000** filen innehåller hello utdata för jobbet.

   > [!NOTE]
   > Vissa MapReduce-jobb kan delas upp hello resultat över flera **del-r-###** filer. I så fall använder hello ### suffix tooindicate hello ordning hello-filer.
   >
   >
5. tooview hello utdata, Använd hello följande kommando:

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    Detta visar en lista över hello ord som ingår i hello **wasb://example/data/gutenberg/davinci.txt** fil och hello antal varje ord inträffade. hello följande är ett exempel på hello data som ska ingå i hello-filen:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Sammanfattning
Som du ser ger hello Hadoop-kommandot ett enkelt sätt toorun MapReduce-jobb på ett HDInsight-kluster och sedan visa hello jobbutdata.

## <a id="nextsteps"></a>Nästa steg
Allmän information om MapReduce-jobb i HDInsight:

* [Använda MapReduce på Hadoop och HDInsight](hdinsight-use-mapreduce.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)
* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)
