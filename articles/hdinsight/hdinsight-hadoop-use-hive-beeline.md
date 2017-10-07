---
title: aaaUse Beeline med Apache Hive - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse hello Beeline klienten toorun Hive-frågor med Hadoop i HDInsight. Beeline är ett verktyg för att arbeta med HiveServer2 över JDBC."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: beeline hive, hive beeline
ms.assetid: 3adfb1ba-8924-4a13-98db-10a67ab24fca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: e788ff39f33d928808cfcb83a92f62ac9ae8ca09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-beeline-client-with-apache-hive"></a>Använda hello Beeline klienten med Apache Hive

Lär dig hur toouse [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) toorun Hive-frågor i HDInsight.

Beeline är en Hive-klient som ingår i hello huvudnoderna av ditt HDInsight-kluster. Beeline använder JDBC tooconnect tooHiveServer2, en tjänst som finns på ditt HDInsight-kluster. Du kan också använda Beeline tooaccess Hive i HDInsight via fjärranslutning över hello internet. hello följande tabell innehåller anslutningssträngar för användning med Beeline:

| Där du kör Beeline från | Parametrar |
| --- | --- | --- |
| En SSH-anslutning tooa headnode eller edge-nod | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
| Utanför hello-kluster | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

> [!NOTE]
> Ersätt `admin` med hello klustret inloggningskonto för klustret.
>
> Ersätt `password` med hello lösenord för hello inloggningskonto för klustret.
>
> Ersätt `clustername` med hello namnet på ditt HDInsight-kluster.

## <a id="prereq"></a>Förhandskrav

* En Linux-baserade Hadoop på HDInsight-kluster.

  > [!IMPORTANT]
  > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* En SSH-klient eller en lokal Beeline-klient. De flesta av hello stegen i det här dokumentet förutsätter att du använder Beeline från ett kluster med SSH-session toohello. Information om hur du kör Beeline från utanför hello-kluster finns i hello [använder Beeline via fjärranslutning](#remote) avsnitt.

    Mer information om hur du använder SSH finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="beeline"></a>Använd Beeline

1. När du startar Beeline, måste du ange en anslutningssträng för HiveServer2 på ditt HDInsight-kluster. toorun hello kommando från utanför hello kluster, måste du även ange hello klusternamnet inloggningen konto (standard `admin`) och lösenord. Använd följande tabell toofind hello anslutning sträng format och parametrar toouse hello:

    | Där du kör Beeline från | Parametrar |
    | --- | --- | --- |
    | En SSH-anslutning tooa headnode eller edge-nod | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
    | Utanför hello-kluster | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

    Följande kommando hello kan till exempel vara används toostart Beeline från ett kluster med SSH-session toohello:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    Detta kommando startar hello Beeline klienten och ansluter tooHiveServer2 i hello klustrets huvudnod. När hello-kommandot har slutförts kommer du till en `jdbc:hive2://headnodehost:10001/>` prompt.

2. Beeline kommandon som börjar med en `!` tecken, till exempel `!help` visar hjälpen. Men hello `!` kan utelämnas för vissa kommandon. Till exempel `help` fungerar även.

    Det finns en `!sql`, vilket är används tooexecute HiveQL-instruktioner. Dock HiveQL så används ofta kan du utelämna hello föregående `!sql`. hello följande två rapporter är likvärdiga:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    Endast en tabell anges på ett nytt kluster: **hivesampletable**.

3. Använd följande kommando toodisplay hello schemat för hello hivesampletable hello:

    ```hiveql
    describe hivesampletable;
    ```

    Det här kommandot returnerar hello följande information:

        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Den här informationen beskriver hello kolumner i hello tabellen. Medan vi kunde utföra några frågor mot dessa data i stället skapar vi en helt ny tabell toodemonstrate hur tooload data till Hive och tillämpa ett schema.

4. Ange hello följande instruktioner toocreate en tabell med namnet **log4jLogs** med exempeldata medföljer hello HDInsight-kluster:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    Dessa instruktioner utför hello följande åtgärder:

    * `DROP TABLE`– Om hello tabellen finns, tas bort.

    * `CREATE EXTERNAL TABLE`-Skapar en **externa** tabellen i Hive. Externa tabeller kan du bara lagra hello tabelldefinition i Hive. hello data finns kvar i hello ursprungliga plats.

    * `ROW FORMAT`-Hur hello data formateras. I det här fallet avgränsas hello fälten i varje logg med ett blanksteg.

    * `STORED AS TEXTFILE LOCATION`-Där hello data lagras och i vilka filformat.

    * `SELECT`-Väljer en uppräkning av alla rader där kolumnen **t4** innehåller hello värdet **[fel]**. Den här frågan returnerar ett värde för **3** som det finns tre rader som innehåller det här värdet.

    * `INPUT__FILE__NAME LIKE '%.log'`-Hive försöker tooapply hello schemafiler tooall i hello directory. I det här fallet innehåller hello katalogen filer som inte matchar hello schemat. tooprevent ogiltiga data i hello resultat instruktionen anger Hive vi bör endast returnera data från filer som slutar på. log.

  > [!NOTE]
  > Externa tabeller ska användas när du förväntar dig hello underliggande data toobe uppdateras av en extern källa. Till exempel en automatisk överföring av data eller en MapReduce-åtgärd.
  >
  > Släppa en extern tabell har **inte** ta bort data hello, bara hello tabelldefinitionen.

    hello utdata från kommandot liknande toohello följer text:

        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :

        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)

        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

5. tooexit Beeline, Använd `!exit`.

## <a id="file"></a>Använd Beeline toorun en HiveQL-fil

Använd följande steg toocreate en fil, kör den med hjälp av Beeline hello.

1. Använd hello följande kommando toocreate en fil med namnet **query.hql**:

    ```bash
    nano query.hql
    ```

2. Använd hello följande text som hello innehållet i hello-fil. Den här frågan skapar en ny ”interna” tabell med namnet **errorLogs**:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Dessa instruktioner utför hello följande åtgärder:

    * **Skapa tabell om inte finns** -om hello tabellen inte redan finns, skapas den. Eftersom hello **externa** nyckelordet används inte, skapar en intern tabell för den här instruktionen. Interna register lagras i datalagret för hello Hive och hanteras helt av Hive.
    * **LAGRAS AS ORC** -lagrar hello data i optimerad raden kolumner (ORC)-format. ORC är ett mycket optimerad och effektiv format för att lagra data med Hive.
    * **INFOGA ÖVER... Välj** -väljer rader från hello **log4jLogs** tabellen som innehåller **[fel]**, och sedan infogningar hello data i hello **errorLogs** tabell.

    > [!NOTE]
    > Till skillnad från externa tabeller kan släppa en intern tabell tar bort hello samt underliggande data.

3. toosave hello-fil, Använd **Ctrl**+**_X**, ange **Y**, och slutligen **RETUR**.

4. Använd följande toorun hello-fil med Beeline hello:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]
    > Hej `-i` parametern börjar Beeline, körs hello instruktioner i hello query.hql fil. När hello frågan har slutförts kan du når hello `jdbc:hive2://headnodehost:10001/>` prompt. Du kan också köra en fil med hello `-f` som avslutas Beeline när hello frågan har slutförts.

5. tooverify hello **errorLogs** tabellen har skapats använder hello följande instruktion tooreturn alla hello rader från **errorLogs**:

    ```hiveql
    SELECT * from errorLogs;
    ```

    Tre raderna med data ska returneras, som innehåller alla **[fel]** i kolumnen t4:

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a id="remote"></a>Använd Beeline via fjärranslutning

Om du har installerat lokalt Beeline eller använder den via en Docker-avbildning som [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/), måste du använda hello följande parametrar:

* __Anslutningssträngen__:`-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`

* __Klustrets inloggningsnamn__:`-n admin`

* __Klustret inloggningslösenordet__`-p 'password'`

Ersätt hello `clustername` i hello-anslutningssträngen med hello namnet på ditt HDInsight-kluster.

Ersätt `admin` med hello namn för klusterinloggning och Ersätt `password` med hello lösenord för kluster-inloggningen.

## <a id="sparksql"></a>Använda Beeline med Spark

Spark innehåller sin egen implementering av HiveServer2, vilket ofta är hänvisade tooas hello Spark Thrift-servern. Den här tjänsten använder Spark SQL tooresolve frågor i stället för Hive och kan ge bättre prestanda beroende på din fråga.

tooconnect toohello Spark Thrift-servern i ett Spark på HDInsight-kluster, Använd port `10002` i stället för `10001`. Till exempel `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.

> [!IMPORTANT]
> hello Spark Thrift-servern är inte direkt tillgänglig över hello internet. Du kan bara ansluta tooit från en SSH-session eller i hello samma virtuella Azure-nätverk som hello HDInsight-kluster.

## <a id="summary"></a><a id="nextsteps"></a>Nästa steg

Mer allmän information om Hive i HDInsight finns i hello följande dokument:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)

Mer information om andra sätt kan du arbeta med Hadoop i HDInsight finns i hello följande dokument:

* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)

Om du använder Tez med Hive finns hello följande dokument:

* [Använda hello Tez UI på Windows-baserade HDInsight](hdinsight-debug-tez-ui.md)
* [Använd hello Ambari Tez vy på Linux-baserat HDInsight](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
