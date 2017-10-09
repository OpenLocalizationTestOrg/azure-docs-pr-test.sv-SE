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
# <a name="run-hello-mapreduce-examples-included-in-hdinsight"></a>Kör hello MapReduce exempel ingår i HDInsight

[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Lär dig hur toorun hello MapReduce exempel ingår i Hadoop i HDInsight.

## <a name="prerequisites"></a>Krav

* **Ett HDInsight-kluster**: finns [komma igång med Hadoop med Hive i HDInsight på Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

    > [!IMPORTANT]
    > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **En SSH-klient**: Mer information finns i [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="hello-mapreduce-examples"></a>Hej MapReduce-exempel

**Plats**: hello-exempel finns på hello HDInsight-kluster på `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.

**Innehållet**: hello följande exempel finns i det här arkivet:

* `aggregatewordcount`: En aggregering baserat mapreduce-program som räknar hello ord i hello indatafiler.
* `aggregatewordhist`: En aggregering baserat mapreduce-program som beräknar hello histogram hello ord i hello indatafiler.
* `bbp`: Ett mapreduce-program som använder Bailey-Borwein-Plouffe toocompute exakta siffror pi.
* `dbcount`: Ett exempel jobb som räknar hello pageview loggar lagras i en databas.
* `distbbp`: Ett mapreduce-program som använder en typ av BBP formeln toocompute exakt bits pi.
* `grep`: Ett mapreduce-program som räknar hello matchar av en regex i hello indata.
* `join`: Ett jobb som utför en koppling över sorteras, lika partitionerad datauppsättningar.
* `multifilewc`: Ett jobb som räknar ord från flera filer.
* `pentomino`: En mapreduce sida vid sida om programmet toofind lösningar toopentomino problem.
* `pi`: Ett mapreduce-program som beräknar Pi med hjälp av en kvasi Monte Carlo-metoden.
* `randomtextwriter`: Ett mapreduce-program som skriver 10 GB av slumpmässiga textdata per nod.
* `randomwriter`: Ett mapreduce-program som skriver 10 GB slumpmässiga data per nod.
* `secondarysort`: Ett exempel som definierar en sekundär sortera toohello minska fasen.
* `sort`: Ett mapreduce-program som sorterar hello data som skrivs av slumpmässiga hello-skrivaren.
* `sudoku`: En sudoku solver.
* `teragen`: Skapa data för hello terasort.
* `terasort`: Kör hello terasort.
* `teravalidate`: Kontrollera resultatet av terasort.
* `wordcount`: Ett mapreduce-program som räknar hello ord i hello indatafiler.
* `wordmean`: Ett mapreduce-program som räknar hello genomsnittliga längden på hello ord i hello indatafiler.
* `wordmedian`: Ett mapreduce-program som räknar hello median längd hello ord i hello indatafiler.
* `wordstandarddeviation`: Ett mapreduce-program som räknar hello standardavvikelsen för hello längd hello ord i hello indatafiler.

**Källkoden**: källkoden för exemplen ingår i HDInsight-kluster på hello `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.

> [!NOTE]
> Hej `2.2.4.9-1` i hello sökväg är hello version av hello Hortonworks Data Platform för hello HDInsight-kluster och kan vara olika för klustret.

## <a name="run-hello-wordcount-example"></a>Kör hello wordcount-exemplet

1. Ansluta tooHDInsight via SSH. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Från hello `username@#######:~$` uppmanar, Använd följande kommandoexempel toolist hello hello:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    Det här kommandot genererar hello lista med exempel från hello föregående avsnitt i det här dokumentet.

3. Använd hello följande kommando tooget hjälp med ett specifikt exempel. I det här fallet hello **wordcount** exempel:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    Hello följande meddelande visas:

        Usage: wordcount <in> [<in>...] <out>

    Det här meddelandet innebär att du kan ange flera indatasökvägarna för hello källdokument. hello slutliga sökvägen är där hello utdata (antal ord i hello källdokument) lagras.

4. Använd följande toocount hello alla ord i hello anteckningsböcker av Leonardo Da Vinci, som tillhandahålls som exempeldata med ditt kluster:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    Indata för det här jobbet har lästs från `/example/data/gutenberg/davinci.txt`. Utdata för det här exemplet är lagrad i `/example/data/davinciwordcount`. Både sökvägar finns på standardlagring för hello kluster inte hello som lokala filsystem.

   > [!NOTE]
   > Enligt beskrivningen i hello hjälp för hello wordcount-exemplet, kan du också ange flera indatafiler. Till exempel `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` räknas orden i både davinci.txt och ulysses.txt.

5. När hello jobbet är slutfört, använder du följande kommandoutdata tooview hello hello:

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    Det här kommandot sammanfogar alla hello utdatafiler som produceras av hello jobb. Den visar hello utdata toohello konsolen. hello utdata är liknande toohello följande text:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Varje rad utgör ett ord och hur många gånger som det har uppstått i hello indata.

## <a name="hello-sudoku-example"></a>Hej Sudoku exempel

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) är logik avgöra består av nio 3 x 3 rutnät. Vissa celler i rutnätet hello har tal, medan andra är tomt och hello målet är toosolve för hello tomma celler. hello föregående länk har mer information om hello problem, men hello syftet med det här exemplet är toosolve för hello tomma celler. Så våra indata ska vara en fil som finns i hello följande format:

* Nio rader nio kolumner
* Varje kolumn kan innehålla antingen ett tal eller `?` (som anger en tom cell)
* Celler avgränsat med ett blanksteg

Det finns ett visst sätt tooconstruct Sudoku kanske; Du kan upprepa ett tal i en kolumn eller rad. Det finns ett exempel på hello HDInsight-kluster som är angett på rätt sätt. Det finns i `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` och innehåller hello följande text:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

toorun problemet exempel via hello Sudoku exempelvis använda hello följande kommando:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

hello sökresultaten liknande toohello följande text:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi--example"></a>Exempel PI (π)

hello pi används en statistisk (kvasi Monte Carlo) metoden tooestimate hello värdet för pi. Punkter placeras slumpmässigt i en ruta för enheten. hello fyrkant innehåller också en cirkel. hello sannolikheten att hello punkter faller inom hello cirkel är lika toohello område i hello cirkel pi/4. hello-värdet för pi beräknas från 4R hello värdet. R är hello förhållandet mellan hello antalet punkter som finns inuti hello cirkel toohello totala antalet punkter som ligger inom hello ruta. hello större hello prov punkter används hello bättre hello uppskattning är.

Använd följande kommando toorun hello det här exemplet. Detta kommando använder 16 maps med 10 000 000 prover tooestimate hello värdet för pi:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

hello värdet som returneras av kommandot är liknande för**3.14159155000000000000**. Referenser är hello första 10 decimaler pi 3.1415926535.

## <a name="10-gb-greysort-example"></a>10 GB Greysort exempel

GraySort är en benchmark-sortering. hello mått är hello sortera hastighet (TB per minut) som uppnås vid sortering av stora mängder data, vanligtvis en 100 TB minsta.

Det här exemplet använder en liten 10 GB data så att den kan köras relativt snabbt. Hej MapReduce program som utvecklats av Owen O'Malley och Arun Murthy används. Dessa program vann hello årliga allmänna (”daytona”) terabyte sortera prestandamått i 2009 med en andel 0.578 TB per minut (100 TB 173 minuter). Mer information om den här och andra sorterings prestandamått finns hello [Sortbenchmark](http://sortbenchmark.org/) plats.

Det här exemplet använder tre olika MapReduce-program:

* **TeraGen**: A MapReduce-program som skapar rader med data toosort

* **TeraSort**: exempel hello indata och använder MapReduce toosort hello data i en total order

    TeraSort är en standard MapReduce-sortering, förutom en anpassad partitioneraren. Hej partitioneraren använder en sorterad lista över N-1 provtagning nycklar som definierar hello viktiga intervallet för varje minska. I synnerhet alla nycklar sådana som exempel [i-1] < = nyckel < exempel [i] skickas tooreduce jag. Den här partitioneraren garanterar att hello utdata för minskar i är mindre än hello utdata från minska i + 1.

* **TeraValidate**: A MapReduce-program som verifierar att hello utdata sorteras globalt

    Det skapar en mappning per fil i hello utdatakatalogen och varje mappning säkerställer att varje nyckel är mindre än eller lika toohello föregående. hello kartan funktionen genererar poster av hello först och senaste nycklarna för varje fil. hello reducera garanterar att hello första nyckeln för filen i är större än hello senaste nyckel i filen i-1. Eventuella problem rapporteras som utdata för hello minska fasen med hello nycklar som är i fel ordning.

Använd hello följande steg toogenerate data, sortera och sedan Validera hello utdata:

1. Generera en 10 GB data som är lagrade toohello HDInsight klustret standardlagring på `/example/data/10GB-sort-input`:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    Hej `-Dmapred.map.tasks` talar om Hadoop hur många kartan uppgifter toouse för det här jobbet. hello sista två parametrar instruera hello jobbet toocreate 10 GB data och toostore den på `/example/data/10GB-sort-input`.

2. Använd följande kommando toosort hello data hello:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    Hej `-Dmapred.reduce.tasks` talar om Hadoop hur många minska uppgifter toouse för hello jobbet. hello sista två parametrar bara hello indata och utdata platser för data.

3. Använd följande toovalidate hello data som genereras av hello sortera hello:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a>Nästa steg

Från den här artikeln får du lära dig hur toorun hello exempel medföljer hello Linux-baserade HDInsight-kluster. Självstudier om hur du använder Pig, finns Hive och MapReduce med HDInsight i hello följande avsnitt:

* [Använda Pig med Hadoop i HDInsight][hdinsight-use-pig]
* [Använda Hive med Hadoop i HDInsight][hdinsight-use-hive]
* [Använda MapReduce med Hadoop i HDInsight][hdinsight-use-mapreduce]

[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
