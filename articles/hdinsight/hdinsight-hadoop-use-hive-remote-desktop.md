---
title: "aaaUse Hadoop Hive och fjärrskrivbord i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur tooconnect tooHadoop kluster i HDInsight med hjälp av fjärrskrivbord och köra Hive-frågor med hjälp av hello Hive kommandoradsgränssnitt."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c228e35-d58a-4f22-917a-1d20c9da89b4
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: f86ffc1be33a8b0b2346d1a1388e5dfa6d0f8777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Använda Hive med Hadoop i HDInsight med fjärrskrivbord
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

I den här artikeln får du lära dig hur tooconnect tooan HDInsight-kluster med hjälp av fjärrskrivbord och sedan köra Hive-frågor med hjälp av hello Hive kommandoradsgränssnittet (CLI).

> [!IMPORTANT]
> Fjärrskrivbord är bara tillgängligt på HDInsight-kluster som använder Windows som hello-operativsystem. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> För HDInsight 3.4 eller större finns [använda Hive med HDInsight och Beeline](hdinsight-hadoop-use-hive-beeline.md) information om hur du kör Hive-frågor direkt på hello kluster från en kommandorad.

## <a id="prereq"></a>Förhandskrav
toocomplete hello stegen i den här artikeln, behöver du hello följande:

* Ett kluster med Windows-baserade HDInsight (Hadoop på HDInsight)
* En klientdator som kör Windows 10, Windows 8 eller Windows 7

## <a id="connect"></a>Ansluta med fjärrskrivbord
Aktivera Fjärrskrivbord för hello HDInsight-kluster och sedan ansluta tooit genom att följa anvisningarna hello på [ansluta tooHDInsight kluster med RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hive"></a>Använd hello Hive-kommando
När du har anslutit toohello desktop för hello HDInsight-kluster Använd följande steg toowork med Hive hello:

1. Starta från hello HDInsight desktop hello **Hadoop kommandoraden**.
2. Ange följande kommando toostart hello Hive CLI hello:

        %hive_home%\bin\hive

    När hello CLI har startats visas hello Hive CLI prompten: `hive>`.
3. Med hjälp av hello CLI, ange hello följande instruktioner toocreate en ny tabell med namnet **log4jLogs** med exempeldata:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Dessa instruktioner utför hello följande åtgärder:

   * **DROP TABLE**: tar bort hello och hello data om hello tabellen finns redan.
   * **Skapa extern tabell**: skapar en ny ”externa” tabell i Hive. Externa tabeller lagra endast hello tabelldefinition i Hive (hello data finns kvar i hello ursprungsplatsen).

     > [!NOTE]
     > Externa tabeller ska användas när du förväntar dig hello underliggande data toobe uppdateras av en extern källa (till exempel en automatisk överföring av data) eller av en annan MapReduce-åtgärd, men du vill använda Hive frågor toouse hello senaste data.
     >
     > Släppa en extern tabell har **inte** ta bort data hello, bara hello tabelldefinitionen.
     >
     >
   * **RADEN FORMAT**: talar om Hive hur hello data formateras. I det här fallet avgränsas hello fälten i varje logg med ett blanksteg.
   * **LAGRAS AS TEXTFILE plats**: talar om Hive där hello data är lagras (hello exempel/datakatalog) och som den lagras som text.
   * **Välj**: väljer en uppräkning av alla rader där kolumnen **t4** innehåller hello värdet **[fel]**. Detta bör returnera ett värde av **3** eftersom det finns tre rader som innehåller det här värdet.
   * **INPUT__FILE__NAME som '%.log'** -talar om Hive som vi ska bara returnera data från filer som slutar på. log. Detta begränsar hello sökning toohello sample.log-fil som innehåller hello data och håller den från att returnera data från andra exempel filer som inte matchar hello schemat som vi har definierat.
4. Använd hello följande instruktioner toocreate en ny ”interna” tabell med namnet **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Dessa instruktioner utför hello följande åtgärder:

   * **Skapa tabell om inte finns**: skapar en tabell om den inte redan finns. Eftersom hello **externa** nyckelordet används inte, det här är en intern tabell som lagras i datalagret för hello Hive och hanteras helt av Hive.

     > [!NOTE]
     > Till skillnad från **externa** tabeller, släppa en intern tabell även tar bort hello underliggande data.
     >
     >
   * **LAGRAS AS ORC**: lagrar hello data i optimerad raden (ORC) kolumnformat. Detta är ett mycket optimerad och effektiv format för att lagra data med Hive.
   * **INFOGA ÖVER... Välj**: väljer rader från hello **log4jLogs** tabellen som innehåller **[fel]**, och sedan infogningar hello data i hello **errorLogs** tabell.

     tooverify som endast rader som innehåller **[fel]** i kolumnen t4 var lagrade toohello **errorLogs** tabell använder hello följande instruktion tooreturn alla hello rader från **errorLogs**:

       Välj * från errorLogs;

     Tre raderna med data ska returneras, som innehåller alla **[fel]** i kolumnen t4.

## <a id="summary"></a>Sammanfattning
Som du ser hello hello Hive kommandot ger ett enkelt sätt toointeractively köra Hive-frågor på ett HDInsight-kluster, övervaka hello jobbstatusen och hämta hello utdata.

## <a id="nextsteps"></a>Nästa steg
Allmän information om Hive i HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)

Om du använder Tez med Hive finns i följande dokument för felsökningsinformation hello:

* [Använda hello Tez UI på Windows-baserade HDInsight](hdinsight-debug-tez-ui.md)
* [Använd hello Ambari Tez vy på Linux-baserat HDInsight](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

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





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
