---
title: "aaaHive med Data Lake (Hadoop) tools för Visual Studio - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toouse hello Data Lake-verktyg för Visual Studio toorun Apache Hive-frågor med Apache Hadoop på Azure HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2b3e672a-1195-4fa5-afb7-b7b73937bfbe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/07/2017
ms.author: larryfr
ms.openlocfilehash: dc76974c02cf68bcf701b2b155842c9e9c5cb988
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hello-data-lake-tools-for-visual-studio"></a>Köra Hive-frågor med hello Data Lake-verktyg för Visual Studio

Lär dig hur toouse hello Data Lake-verktyg för Visual Studio tooquery Apache Hive. hello Data Lake-verktyg kan du tooeasily skapa, skicka och övervaka Hive-frågor tooHadoop på Azure HDInsight.

## <a id="prereq"></a>Förhandskrav

* Ett kluster i Azure HDInsight (Hadoop på HDInsight)

  > [!IMPORTANT]
  > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio (ett av följande versioner hello):

    * Visual Studio Community 2013/Professional/Premium/Ultimate med Update 4

    * Visual Studio 2015 (någon utgåva)

    * 2017 för Visual Studio (någon utgåva)

* HDInsight tools för Visual Studio eller Azure Data Lake-verktyg för Visual Studio. Se [komma igång med Visual Studio Hadoop-verktygen för HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) för information om hur du installerar och konfigurerar hello verktyg.

## <a id="run"></a>Köra Hive-frågor med hello Visual Studio

1. Öppna **Visual Studio** och välj **ny** > **projekt** > **Azure Data Lake** > **HIVE** > **Hive-program**. Ange ett namn för det här projektet.

2. Öppna hello **Script.hql** filen som skapas med det här projektet och klistra in följande HiveQL-instruktioner hello:

   ```hiveql
   set hive.execution.engine=tez;
   DROP TABLE log4jLogs;
   CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
   STORED AS TEXTFILE LOCATION '/example/data/';
   SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   ```

    Dessa instruktioner utför hello följande åtgärder:

   * `DROP TABLE`: Om hello tabellen finns den här instruktionen tar bort den.

   * `CREATE EXTERNAL TABLE`: Skapar en ny ”externa” tabell i Hive. Externa tabeller kan du bara lagra hello tabelldefinition i Hive (hello data finns kvar i hello ursprungsplatsen).

     > [!NOTE]
     > Externa tabeller ska användas när du förväntar dig hello underliggande data toobe uppdateras av en extern källa. Till exempel en MapReduce-jobb eller Azure-tjänsten.
     >
     > Släppa en extern tabell har **inte** ta bort data hello, bara hello tabelldefinitionen.

   * `ROW FORMAT`: Anger hur hello data är formaterad Hive. I det här fallet avgränsas hello fälten i varje logg med ett blanksteg.

   * `STORED AS TEXTFILE LOCATION`: Talar om Hive där hello data lagras (hello exempel/datakatalog) och att den lagras som text.

   * `SELECT`: Välj en uppräkning av alla rader där kolumnen `t4` innehåller hello värdet `[ERROR]`. Den här instruktionen returnerar ett värde för `3` eftersom det finns tre rader som innehåller det här värdet.

   * `INPUT__FILE__NAME LIKE '%.log'`-Talar om Hive vi bör endast returnera data från filer som slutar på. log. Den här satsen begränsar hello sökning toohello sample.log-fil som innehåller hello data.

3. Välj hello hello verktygsfältet **HDInsight-kluster** som du vill toouse för den här frågan. Välj **skicka** toorun hello uttrycken som ett Hive-jobb.

   ![Skicka stapel](./media/hdinsight-hadoop-use-hive-visual-studio/toolbar.png)

4. Hej **Hive-jobbsammanfattning** visas information om hello köra jobb. Använd hello **uppdatera** länka toorefresh hello jobbinformation tills hello **jobbstatus** ändras för**slutförd**.

   ![jobbsammanfattning visa slutförda jobb](./media/hdinsight-hadoop-use-hive-visual-studio/jobsummary.png)

5. Använd hello **Jobbutdata** länka tooview hello utdata för jobbet. Den visar `[ERROR] 3`, vilket är hello-värdet som returneras av den här frågan.

6. Du kan också köra Hive-frågor utan att skapa ett projekt. Med hjälp av **Server Explorer**, expandera **Azure** > **HDInsight**, högerklicka på ditt HDInsight-servern och välj sedan **skriva en Hive-fråga**.

7. I hello **temp.hql** dokument som visas, Lägg till följande HiveQL-instruktioner hello:

   ```hiveql
   set hive.execution.engine=tez;
   CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
   INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   ```

    Dessa instruktioner utför hello följande åtgärder:

   * `CREATE TABLE IF NOT EXISTS`: Skapar en tabell om den inte redan finns. Eftersom hello `EXTERNAL` nyckelordet används inte, skapar en intern tabell för den här instruktionen. Interna register lagras i datalagret för hello Hive och hanteras av Hive.

     > [!NOTE]
     > Till skillnad från `EXTERNAL` tabeller, släppa en intern tabell även tar bort hello underliggande data.

   * `STORED AS ORC`: Lagrar hello data i optimerad raden (ORC) kolumnformat. ORC är ett mycket optimerad och effektiv format för att lagra data med Hive.

   * `INSERT OVERWRITE ... SELECT`: Om du väljer rader från hello `log4jLogs` tabellen som innehåller `[ERROR]`, och sedan infogningar hello data i hello `errorLogs` tabell.

8. Hello-verktygsfältet, Välj **skicka** toorun hello jobb. Använd hello **jobbstatus** toodetermine hello jobbet har slutförts.

9. tooverify som hello jobbet skapat hello tabell, Använd **Server Explorer** och expandera **Azure** > **HDInsight** > ditt HDInsight-kluster > **Hive-databaser** > **standard**. Hej **errorLogs** tabell och hello **log4jLogs** tabellen visas.

## <a id="nextsteps"></a>Nästa steg

Som du ser tillhandahåller hello HDInsight tools för Visual Studio ett enkelt sätt toowork med Hive-frågor på HDInsight.

Allmän information om Hive i HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)

* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)

Mer information om hello HDInsight tools för Visual Studio:

* [Komma igång med HDInsight tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md)

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

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
