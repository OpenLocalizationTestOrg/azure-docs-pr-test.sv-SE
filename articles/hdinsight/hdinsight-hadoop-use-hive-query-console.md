---
title: "aaaUse Hadoop Hive på hello frågan konsolen i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse hello webbaserade konsolen frågan toorun Hive-frågor på ett HDInsight Hadoop-kluster från din webbläsare."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5ae074b0-f55e-472d-94a7-005b0e79f779
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 621882082c9a07655d34b8dc980b8e47dd04b745
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hello-query-console"></a>Köra Hive-frågor med hello frågan konsolen
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

I den här artikeln får du lära dig hur toouse hello HDInsight frågan konsolen toorun Hive-frågor på ett HDInsight Hadoop-kluster från din webbläsare.

> [!IMPORTANT]
> Hej HDInsight frågan konsolen är bara tillgängligt på Windows-baserade HDInsight-kluster. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> För HDInsight 3.4 eller större finns [köra Hive-frågor i Ambari Hive-vy](hdinsight-hadoop-use-hive-ambari-view.md) information om hur du kör Hive-frågor från en webbläsare.

## <a id="prereq"></a>Förhandskrav
toocomplete hello stegen i den här artikeln, behöver du hello följande.

* Ett Windows-baserade HDInsight Hadoop-kluster
* En modern webbläsare

## <a id="run"></a>Köra Hive-frågor med hello frågan konsolen
1. Öppna en webbläsare och gå för**https://CLUSTERNAME.azurehdinsight.net**, där **KLUSTERNAMN** är hello namnet på ditt HDInsight-kluster. Om du uppmanas ange hello användarnamn och lösenord som du använde när du skapade hello.
2. Hello länkar hello överst på hello sidan, Välj **Hive-redigeraren**. Detta visar ett formulär som kan använda tooenter hello HiveQL-instruktioner som du vill toorun i hello HDInsight-kluster.

    ![hello hive-redigeraren](./media/hdinsight-hadoop-use-hive-query-console/queryconsole.png)

    Ersätt texten hello `Select * from hivesampletable` med hello följande HiveQL-instruktioner:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Dessa instruktioner utför hello följande åtgärder:

   * **DROP TABLE**: tar bort hello och hello data om hello tabellen finns redan.
   * **Skapa extern tabell**: skapar en ny ”externa” tabell i Hive. Externa tabeller lagra endast hello tabelldefinition i Hive; hello data finns kvar i hello ursprungliga plats.

     > [!NOTE]
     > Externa tabeller ska användas när du förväntar dig hello underliggande data toobe uppdateras av en extern källa (till exempel en automatisk överföring av data) eller av en annan MapReduce-åtgärd, men du vill använda Hive frågor toouse hello senaste data.
     >
     > Släppa en extern tabell har **inte** ta bort data hello, bara hello tabelldefinitionen.
     >
     >
   * **RADEN FORMAT**: talar om Hive hur hello data formateras. I det här fallet avgränsas hello fälten i varje logg med ett blanksteg.
   * **LAGRAS AS TEXTFILE plats**: talar om Hive där hello data är lagras (hello exempel/datakatalog) och som den lagras som text
   * **Välj**: Välj en uppräkning av alla rader där kolumnen **t4** innehåller hello värde **[fel]**. Detta bör returnera ett värde av **3** eftersom det finns tre rader som innehåller det här värdet.
   * **INPUT__FILE__NAME som '%.log'** -talar om Hive som vi ska bara returnera data från filer som slutar på. log. Detta begränsar hello sökning toohello sample.log-fil som innehåller hello data och håller den från att returnera data från andra exempel filer som inte matchar hello schemat som vi har definierat.
3. Klicka på **skicka**. Hej **jobbet Session** på hello längst ned på sidan hello ska visa information om hello jobb.
4. När hello **Status** fältet ändringar för**slutförd**väljer **visa information om** för hello jobbet. Hello information på sidan hello **Jobbutdata** innehåller `[ERROR]    3`. Du kan använda hello **hämta** knappen under det här fältet toodownload en fil som innehåller hello utdata för hello jobb.

## <a id="summary"></a>Sammanfattning
Som du ser hello frågan konsolen ger ett enkelt sätt toorun Hive-frågor i ett HDInsight-kluster, övervaka hello jobbstatus och hämta hello utdata.

toolearn mer information om hur du använder Hive-fråga konsolen toorun Hive-jobb, Välj **komma igång** överst hello i hello frågan konsolen, sedan använda hello-exempel som tillhandahålls. Varje prov går igenom hello processen med att använda Hive tooanalyze data, inklusive beskrivningar av hello HiveQL-instruktioner som används i hello exempel.

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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
