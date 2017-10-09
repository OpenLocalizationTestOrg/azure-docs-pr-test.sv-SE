---
title: aaaUse Ambari Views toowork med Hive i HDInsight (Hadoop) - Azure | Microsoft Docs
description: "Lär dig hur toouse hello Hive-vy från dina webb webbläsare toosubmit Hive-frågor. hello Hive-vy är en del av hello Ambari-Webbgränssnittet medföljer ditt Linux-baserade HDInsight-kluster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1abe9104-f4b2-41b9-9161-abbc43de8294
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: f9a77b652e70d34a0ff9165fbb8c2e16d3401ae0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-hive-view-with-hadoop-in-hdinsight"></a>Använda hello Hive-vy med Hadoop i HDInsight

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Lär dig hur toorun Hive-frågor med Ambari Hive-vy. Ambari är ett hantering och övervakning verktyg som medföljer Linux-baserade HDInsight-kluster. En av hello-funktioner som tillhandahålls via Ambari är ett Webbgränssnitt som kan använda toorun Hive-frågor.

> [!NOTE]
> Ambari har många funktioner som inte beskrivs i det här dokumentet. Mer information finns i [hantera HDInsight-kluster med hjälp av hello Ambari-Webbgränssnittet](hdinsight-hadoop-manage-ambari.md).

## <a name="prerequisites"></a>Krav

* Ett Linux-baserat HDInsight-kluster. Information om hur du skapar klustret finns [komma igång med Linux-baserade HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

> [!IMPORTANT]
> hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="open-hello-hive-view"></a>Öppna hello Hive-vyn

Du kan Ambari-vyer från hello Azure-portalen; Välj ditt HDInsight-kluster och välj sedan **Ambari Views** från hello **snabblänkar** avsnitt.

![Snabblänkar hello-portalen](./media/hdinsight-hadoop-use-hive-ambari-view/quicklinks.png)

Välj hello hello listan vyer __Hive-vy__.

![hello Hive-vy markerat](./media/hdinsight-hadoop-use-hive-ambari-view/select-hive-view.png)

> [!NOTE]
> När åt Ambari, kan du ange tooauthenticate toohello plats. Ange Hej administratör (standard `admin`) konto och lösenord som du använde när du skapar hello klustret.

Du bör se en sida liknande toohello följande bild:

![Bild av hello frågan kalkylblad för hello Hive-vyn](./media/hdinsight-hadoop-use-hive-ambari-view/ambari-hive-view.png)

## <a name="hivequery"></a>Köra en fråga

toorun en hive-fråga använda hello följande från hello Hive-vyn.

1. Från hello __frågan__ och klistra in följande HiveQL-instruktioner i kalkylbladet med hello hello:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
    ```

    Dessa instruktioner utför hello följande åtgärder:

   * `DROP TABLE`-Tar bort hello tabell och hello datafilen hello tabellen redan finns.

   * `CREATE EXTERNAL TABLE`– Skapar en ny ”externa” tabell i Hive.
   Externa tabeller lagra endast hello tabelldefinition i Hive. hello data finns kvar i hello ursprungliga plats.

   * `ROW FORMAT`-Hur hello data formateras. I det här fallet avgränsas hello fälten i varje logg med ett blanksteg.

   * `STORED AS TEXTFILE LOCATION`-Där hello data lagras och som det lagras som text.

   * `SELECT`-Väljer en uppräkning av alla rader där kolumnen t4 innehåller hello värde [fel].

     > [!NOTE]
     > Externa tabeller ska användas när du förväntar dig hello underliggande data toobe uppdateras av en extern källa. Till exempel en automatiserad dataöverföringen process, eller av en annan MapReduce-åtgärd. Släppa en extern tabell har *inte* ta bort data hello, bara hello tabelldefinitionen.

    > [!IMPORTANT]
    > Lämna hello __databasen__ valet __standard__. hello exemplen i det här dokumentet används hello standarddatabasen som ingår i HDInsight.

2. toostart hello fråga, Använd hello **Execute** nedan hello kalkylblad. Den blir orange och hello textändringar för**stoppa**.

3. När hello frågan har slutförts hello **resultat** visar hello resultaten av hello igen. hello efter texten är hello resultatet av hello fråga:

        sev       cnt
        [ERROR]   3

    Hej **loggar** flik kan vara används tooview hello loggningsinformation som skapats av hello-jobbet.

   > [!TIP]
   > Hej **spara resultaten** listrutan dialogrutan i hello övre vänstra hörnet hello **Frågeprocessresultat** avsnitt kan du toodownload eller spara resultaten.

4. Välj hello fyra första raderna i den här frågan väljer **Execute**. Observera att det finns inga resultat när hello jobbet har slutförts. Med hjälp av hello **Execute** knappen när det ingår i hello frågan markeras bara körs hello markerade rapporter. I det här fallet skickat hello markeringen hello sista instruktionen som hämtar rader från hello tabell. Om du väljer bara den raden och använda **kör**, bör du se hello förväntat resultat.

5. tooadd ett kalkylblad använder hello **nytt kalkylblad** knappen längst ned hello hello **frågeredigeraren**. Ange hello följande HiveQL-instruktioner i hello nytt kalkylblad:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
    ```

  Dessa instruktioner utför hello följande åtgärder:

   * **Skapa tabell om inte finns** -skapar en tabell om den inte redan finns. Eftersom hello **externa** nyckelordet används inte, en intern tabell skapas. En intern tabell lagras i datalagret för hello Hive och hanteras helt av Hive. Till skillnad från externa tabeller kan släppa en intern tabell tar bort hello samt underliggande data.

   * **LAGRAS AS ORC** -lagrar hello data i optimerad raden kolumner (ORC)-format. ORC är ett mycket optimerad och effektiv format för att lagra data med Hive.

   * **INFOGA ÖVER... Välj** -väljer rader från hello **log4jLogs** tabellen som innehåller `[ERROR]`, och sedan infogningar hello data i hello **errorLogs** tabell.

     Använd hello **Execute** knappen toorun den här frågan. Hej **resultat** fliken innehåller inte någon information när hello frågan returnerar noll rader. hello status ska visa **lyckades** när hello frågan har slutförts.

### <a name="visual-explain"></a>Visual förklarar

toodisplay en visualisering av hello frågeplan, Välj hello **Visual förklarar** tabbtangenten hello kalkylblad.

Hej **Visual förklarar** vy över hello frågan kan vara lättare att förstå hello flödet av komplexa frågor. Du kan visa en textrepresentation motsvarigheten till den här vyn genom att använda hello **förklara** knapp i hello frågeredigeraren.

### <a name="tez-ui"></a>Tez-Gränssnittet

toodisplay hello Tez UI hello frågan, Välj hello **Tez** tabbtangenten hello kalkylblad.

> [!IMPORTANT]
> Tez är används inte tooresolve alla frågor. Många frågor kan lösas utan att använda Tez. 

Om Tez var används tooresolve hello fråga, hello dirigeras acykliska diagram (DAG) visas. Om du vill tooview hello DAG för frågor som du har kört i hello senaste eller felsöka hello Tez-processen, Använd hello [Tez visa](hdinsight-debug-ambari-tez-view.md) i stället.

## <a name="view-job-history"></a>Visa jobbhistorik

Hej __jobb__ visar en historik över Hive-frågor.

![Bild av hello jobbhistorik](./media/hdinsight-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a>Databastabeller

Du kan använda hello __tabeller__ fliken toowork med tabeller i en databas för Hive.

![Bild av hello tabeller fliken](./media/hdinsight-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a>Sparade frågor

Du kan du kan också spara frågor från hello frågan på fliken. När du sparat du kan återanvända hello frågan från hello __sparade frågor__ fliken.

![Bild av fliken sparade frågor](./media/hdinsight-hadoop-use-hive-ambari-view/saved-queries.png)

## <a name="user-defined-functions"></a>Användardefinierade funktioner

Hive utökas även via användardefinierade funktioner (UDF). En UDF kan du tooimplement funktioner eller som inte är enkelt modelleras i HiveQL.

hello UDF fliken hello överst i hello Hive-vy kan du toodeclare och spara en uppsättning UDF: er. Dessa UDF: er kan användas med hello **frågeredigeraren**.

![Bild av UDF-fliken](./media/hdinsight-hadoop-use-hive-ambari-view/user-defined-functions.png)

När du har lagt till UDF-toohello Hive-vy, en **Infoga UDF: er** visas knappen längst ned hello hello **frågeredigeraren**. Om du markerar den här posten visas listrutan för hello UDF: er som definierats i hello Hive-vy. Markerar en UDF läggs HiveQL-instruktioner tooyour frågan tooenable hello UDF.

Till exempel om du har definierat en UDF med hello följande egenskaper:

* Resursnamnet: myudfs

* Resursens sökväg: /myudfs.jar

* UDF-namn: myawesomeudf

* Klassnamn för UDF: com.myudfs.Awesome

Med hjälp av hello **Infoga UDF: er** knappen visar en post med namnet **myudfs**, med en annan listrutan för varje UDF har definierats för den här resursen. I det här fallet **myawesomeudf**. Markerar den här posten läggs hello efter toohello början av hello-frågan:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

Du kan sedan använda hello UDF i frågan. Till exempel `SELECT myawesomeudf(name) FROM people;`.

Mer information om hur du använder UDF: er med Hive i HDInsight finns i följande dokument hello:

* [Med hjälp av Python med Hive och Pig i HDInsight](hdinsight-python.md)
* [Hur tooadd en anpassad Hive UDF-tooHDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a>Hive-inställningar

Inställningarna kan vara används toochange olika Hive-inställningar. Till exempel ändra hello Körningsmotor för Hive från tooMapReduce Tez (hello standard).

## <a id="nextsteps"></a>Nästa steg

Allmän information om Hive i HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)
