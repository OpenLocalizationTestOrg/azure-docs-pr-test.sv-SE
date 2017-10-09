---
title: "aaaWhat är Apache Hive och HiveQL - Azure HDInsight | Microsoft Docs"
description: "Apache Hive är ett data warehouse system för Hadoop. Du kan fråga data som lagras i Hive med HiveQL, liknande tooTransact-SQL. I detta dokument, lär du dig hur toouse Hive och HiveQL med Azure HDInsight."
keywords: "hiveql, vad är hive, hadoop hiveql hur toouse hive Läs hive, vad är hive"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2c10f989-7636-41bf-b7f7-c4b67ec0814f
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: a2772312263895ff99b499898264c2e6d5e816e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a>Vad är Apache Hive och HiveQL på Azure HDInsight?

[Apache Hive](http://hive.apache.org/) är ett data warehouse system för Hadoop. Hive kan sammanfatta data, frågor och analys av data. Hive-frågor är skrivna i HiveQL, vilket är en liknande tooSQL språk för frågan.

Hive tillåter tooproject strukturen på i stort sett Ostrukturerade data. När du har definierat strukturen hello kan du använda HiveQL tooquery hello data utan kännedom om Java eller MapReduce.

HDInsight tillhandahåller flera klustertyper som är anpassade för specifika arbetsbelastningar. följande klustertyper hello används oftast för Hive-frågor:

* __Interaktiva Hive__: A Hadoop-kluster som innehåller [låg latens analytisk behandling (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) funktioner tooimprove svarstider för interaktiva frågor. Mer information finns i hello [börja med interaktiva Hive i HDInsight](hdinsight-hadoop-use-interactive-hive.md) dokumentet.

* __Hadoop__: A Hadoop-kluster som är anpassad för batchbearbetning arbetsbelastningar. Mer information finns i hello [starta med Hadoop i HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) dokumentet.

* __Spark__: Apache Spark har inbyggda funktioner för att arbeta med Hive. Mer information finns i hello [börja med Spark i HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) dokumentet.

* __HBase__: HiveQL kan vara används tooquery data som lagras i HBase. Mer information finns i hello [börjar med HBase på HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) dokumentet.

## <a name="how-toouse-hive"></a>Hur toouse Hive

Använd följande tabell toodiscover hur toouse Hive med HDInsight hello:

| **Använd den här metoden** om du vill... | .. .an **interaktiva** shell | ... **batch** bearbetning | ... med detta **klustret operativsystem** | .. .from detta **klientoperativsystem** |
|:--- |:---:|:---:|:--- |:--- |
| [Hive-vyn](hdinsight-hadoop-use-hive-ambari-view.md) |✔ |✔ |Linux |Alla (webbläsarbaserade) |
| [Beeline klienten](hdinsight-hadoop-use-hive-beeline.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X eller Windows |
| [REST-API](hdinsight-hadoop-use-hive-curl.md) |&nbsp; |✔ |Linux- eller Windows * |Linux, Unix, Mac OS X eller Windows |
| [HDInsight tools för Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |&nbsp; |✔ |Linux- eller Windows * |Windows |
| [Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md) |&nbsp; |✔ |Linux- eller Windows * |Windows |

> [!IMPORTANT]
> \*Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Om du använder ett Windows-baserade HDInsight-kluster kan du använda hello [frågan konsolen](hdinsight-hadoop-use-hive-query-console.md) från din webbläsare eller [fjärrskrivbord](hdinsight-hadoop-use-hive-remote-desktop.md) toorun Hive-frågor.

## <a name="hiveql-language-reference"></a>Språkreferens för HiveQL

Språkreferens för HiveQL är tillgängliga i hello [språk manuell (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).

## <a name="hive-and-data-structure"></a>Hive och data struktur

Hive förstår hur toowork med strukturerade och halvstrukturerade data. Till exempel textfiler där hello fälten avgränsas med tecken. hello följande HiveQL-uttrycket skapar en tabell över blankstegsavgränsad data:

```hiveql
CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

Hive även stöder anpassad **serialiseraren/deserializers (SerDe)** för komplex eller oregelbundet strukturerade data. Mer information finns i hello [hur toouse en anpassad JSON-SerDe med HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) dokumentet.

Mer information om filformat som stöds av Hive finns hello [Language manual (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)

## <a name="hive-internal-tables-vs-external-tables"></a>Hive interna register vs externa tabeller

Det finns två typer av tabeller som du kan skapa med Hive:

* __Internt__: Data lagras i datalagret för hello Hive. hello datalagret finns i `/hive/warehouse/` på hello standardlagring för hello-kluster.

    Använder interna tabeller när:

    * Data är tillfällig.
    * Vill du Hive toomanage hello livscykeln för hello tabellen och data.

* __Externa__: Data lagras utanför hello-datalagret. hello data kan lagras på all tillgänglig lagring av hello-kluster.

    Använd externa tabeller när:

    * hello data används också utanför Hive. Till exempel har hello datafiler uppdaterats av en annan process (som inte låser hello filer.)
    * Data måste tooremain i hello underliggande plats, även efter att släppa hello tabell.
    * Du behöver en anpassad plats, till exempel ett lagringskonto som inte är standard.
    * Ett annat program än hive hanterar hello dataformat, platsen, osv.

Mer information finns i hello [Hive interna och externa tabeller introduktion] [ cindygross-hive-tables] blogginlägg.

## <a name="user-defined-functions-udf"></a>Användardefinierade funktioner (UDF)

Hive kan också utökas genom **användardefinierade funktioner (UDF)**. En UDF kan du tooimplement funktioner eller som inte är enkelt modelleras i HiveQL. Ett exempel på hur UDF: er med Hive finns hello följande dokument:

* [Använda en användardefinierad funktion i Java med Hive](hdinsight-hadoop-hive-java-udf.md)

* [Använda Python användardefinierad funktion med Hive och Pig](hdinsight-python.md)

* [Använda en C# användardefinierad funktion med Hive och Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Hur tooadd anpassade registreringsdata användardefinierade funktionen tooHDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Ett exempel Hive användardefinierad funktion tooconvert datum/tid-format tooHive tidsstämpel](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a id="data"></a>Exempeldata

Hive i HDInsight levereras förinstallerade med en intern tabell med namnet `hivesampletable`. HDInsight tillhandahåller även exempel datamängder som kan användas med Hive. Dessa uppgifter som lagras i hello `/example/data` och `/HdiSamples` kataloger. Dessa kataloger finns i hello standardlagring för klustret.

## <a id="job"></a>Exempel Hive-fråga

Hej följande HiveQL-instruktioner projektkolumner till hello `/example/data/sample.log` fil:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

I föregående exempel hello utföra hello HiveQL-instruktioner hello följande åtgärder:

* `set hive.execution.engine=tez;`: Uppsättningarna hello körning av motorn toouse Tez. Tez ger i stället för MapReduce en ökning i frågeprestanda. Mer information om Tez finns hello [använda Apache Tez för bättre prestanda](#usetez) avsnitt.

    > [!NOTE]
    > Den här instruktionen är bara krävs när du använder ett Windows-baserade HDInsight-kluster. Tez är motorn för körning av hello standard för Linux-baserade HDInsight.

* `DROP TABLE`: Ta bort om hello tabellen redan finns.

* `CREATE EXTERNAL TABLE`: Skapar en ny **externa** tabellen i Hive. Externa tabeller kan du bara lagra hello tabelldefinition i Hive. hello data finns kvar i hello ursprungliga plats och hello ursprungliga format.

* `ROW FORMAT`: Anger hur hello data är formaterad Hive. I det här fallet avgränsas hello fälten i varje logg med ett blanksteg.

* `STORED AS TEXTFILE LOCATION`: Talar om Hive där hello data lagras (hello `example/data` directory) och att den lagras som text. hello data kan vara i en fil eller sprida över flera filer i hello katalog.

* `SELECT`: Väljer en uppräkning av alla rader där hello kolumnen **t4** innehåller hello värdet **[fel]**. Den här instruktionen returnerar ett värde för **3** eftersom det finns tre rader som innehåller det här värdet.

* `INPUT__FILE__NAME LIKE '%.log'`-Hive försöker tooapply hello schemafiler tooall i hello directory. I det här fallet innehåller hello katalogen filer som inte matchar hello schemat. tooprevent ogiltiga data i hello resultat instruktionen anger Hive vi bör endast returnera data från filer som slutar på. log.

> [!NOTE]
> Externa tabeller ska användas när du förväntar dig hello underliggande data toobe uppdateras av en extern källa. Till exempel en automatisk överföring av data eller MapReduce-åtgärden.
>
> Släppa en extern tabell har **inte** bort hello data tar endast bort hello tabelldefinitionen.

toocreate en **interna** tabellen i stället för externa använder hello följande HiveQL:

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Dessa instruktioner utför hello följande åtgärder:

* `CREATE TABLE IF NOT EXISTS`: Skapa den om hello tabellen inte finns. Eftersom hello **externa** nyckelordet används inte, skapar en intern tabell för den här instruktionen. hello tabell lagras i datalagret för hello Hive och hanteras helt av Hive.

* `STORED AS ORC`: Lagrar hello data i optimerad raden kolumner (ORC)-format. ORC är ett mycket optimerad och effektiv format för att lagra data med Hive.

* `INSERT OVERWRITE ... SELECT`: Om du väljer rader från hello **log4jLogs** tabell som innehåller **[fel]**, och sedan infogningar hello data i hello **errorLogs** tabell.

> [!NOTE]
> Till skillnad från externa tabeller kan släppa en intern tabell tas även bort hello underliggande data.

## <a name="improve-hive-query-performance"></a>Förbättra prestanda för Hive-frågor

### <a id="usetez"></a>Apache Tez

[Apache Tez](http://tez.apache.org) är ett ramverk som gör att dataintensiva applikationer, som Hive, toorun mer effektivt i större skala. Tez är aktiverad som standard för Linux-baserade HDInsight-kluster.

> [!NOTE]
> Tez är inaktiverat som standard för Windows-baserade HDInsight-kluster och den måste aktiveras. tootake nytta av Tez hello följande värde måste anges för en Hive-fråga:
>
> `set hive.execution.engine=tez;`
>
> Tez är hello standard motor för Linux-baserade HDInsight-kluster.

Hej [Hive i Tez designdokument](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) innehåller information om hello implementering alternativ och prestandajustering konfigurationer.

tooaid felsöka jobb kördes med Tez, HDInsight tillhandahåller följande web-användargränssnitt som gör att du tooview information om jobb på Tez hello:

* [Använd hello Ambari Tez vy på Linux-baserat HDInsight](hdinsight-debug-ambari-tez-view.md)

* [Använda hello Tez UI på Windows-baserade HDInsight](hdinsight-debug-tez-ui.md)

### <a name="low-latency-analytical-processing-llap"></a>Låg latens Analytical Processing (LLAP)

[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (kallas ibland Live långa och processen) är en ny funktion i Hive 2.0 som kan cachelagra i minnet för frågor. LLAP gör Hive-frågor som är mycket snabbare upp för[26 x snabbare än Hive 1.x i vissa fall](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).

HDInsight ger LLAP hello interaktiva Hive typ av kluster. Mer information finns i hello [börja med interaktiva Hive](hdinsight-hadoop-use-interactive-hive.md) dokumentet.

## <a name="hive-jobs-and-sql-server-integration-services"></a>Hive-jobb och SQL Server Integration Services

Du kan använda SQL Server Integration Services (SSIS) toorun ett Hive-jobb. hello Azure Funktionspaket för SSIS tillhandahåller hello följande komponenter som arbetar med Hive-jobb i HDInsight.

* [Azure HDInsight Hive-aktivitet][hivetask]

* [Azure prenumeration Anslutningshanteraren][connectionmanager]

Läs mer om hello Azure Funktionspaket för SSIS [här][ssispack].

## <a id="nextsteps"></a>Nästa steg

Nu när du har lärt dig Hive är och hur toouse den med Hadoop i HDInsight, Använd hello följande länkar tooexplore andra sätt toowork med Azure HDInsight.

* [Ladda upp data tooHDInsight][hdinsight-upload-data]
* [Använda Pig med HDInsight][hdinsight-use-pig]
* [Använda MapReduce-jobb med HDInsight][hdinsight-use-mapreduce]

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
