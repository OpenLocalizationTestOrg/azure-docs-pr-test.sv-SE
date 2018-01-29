---
title: "Använda Ambari-vyer för att arbeta med Hive i HDInsight (Hadoop) - Azure | Microsoft Docs"
description: "Lär dig hur du använder Hive-vy från webbläsaren för att skicka Hive-frågor. Hive-vy är en del av Ambari-Webbgränssnittet som medföljer ditt Linux-baserade HDInsight-kluster."
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
ms.date: 01/19/2018
ms.author: larryfr
ms.openlocfilehash: 5f66e60249af489e695029cbb072f3cc881bb039
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/20/2018
---
# <a name="use-ambari-hive-view-with-hadoop-in-hdinsight"></a>Använda Ambari Hive-vyn med Hadoop i HDInsight

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Lär dig hur du kör Hive-frågor genom att använda Ambari Hive-vy. Ambari är ett hantering och övervakning verktyg som medföljer Linux-baserade HDInsight-kluster. En av de funktioner som tillhandahålls via Ambari är ett Webbgränssnitt som kan användas för att köra Hive-frågor.

> [!NOTE]
> Ambari har många funktioner som inte beskrivs i det här dokumentet. Mer information finns i [hantera HDInsight-kluster med Ambari-Webbgränssnittet](../hdinsight-hadoop-manage-ambari.md).

## <a name="prerequisites"></a>Förutsättningar

* Ett Linux-baserat HDInsight-kluster. Information om hur du skapar kluster finns i [komma igång med Hadoop i HDInsight](apache-hadoop-linux-tutorial-get-started.md).

> [!IMPORTANT]
> Stegen i det här dokumentet kräver ett Azure HDInsight-kluster som använder Linux. Linux är det enda operativsystem som används i HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="open-the-hive-view"></a>Öppna Hive-vyn

Du kan öppna Ambari-vyer från Azure-portalen. Välj ditt HDInsight-kluster och välj sedan **Ambari Views** från den **snabblänkar** avsnitt.

![Snabblänkar av portalen](./media/apache-hadoop-use-hive-ambari-view/quicklinks.png)

Välj i listan över vyer __Hive-vy__.

![Den valda Hive-vyn](./media/apache-hadoop-use-hive-ambari-view/select-hive-view.png)

> [!NOTE]
> När du försöker komma åt Ambari, uppmanas du att autentisera till platsen. Ange administratören (standard `admin`) konto och lösenord som du använde när du skapade klustret.

Du bör se en sida som liknar följande bild:

![Bild av frågan kalkylbladet för Hive-vy](./media/apache-hadoop-use-hive-ambari-view/ambari-hive-view.png)

## <a name="run-a-query"></a>Köra en fråga

Använd följande steg från Hive-vyn för att köra en Hive-fråga.

1. Från den __frågan__ och klistra in följande HiveQL-instruktioner i kalkylbladet:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
    ```

    Dessa instruktioner utför följande åtgärder:

   * `DROP TABLE`: Tar bort tabellen och datafilen, om tabellen redan finns.

   * `CREATE EXTERNAL TABLE`: Skapar en ny ”externa” tabell i Hive.
   Externa tabeller lagra endast tabelldefinitionen i Hive. Data finns kvar i den ursprungliga platsen.

   * `ROW FORMAT`: Visar hur data ska formateras. I det här fallet avgränsas fälten i varje logg med ett blanksteg.

   * `STORED AS TEXTFILE LOCATION`: Visar var data lagras och som det lagras som text.

   * `SELECT`: Väljer en uppräkning av alla rader där kolumnen t4 innehåller värdet [fel].

     > [!NOTE]
     > Använda externa tabeller när du förväntar dig underliggande data uppdateras av en extern källa, överföra, till exempel en automatiserad process eller en annan MapReduce-åtgärd. Släppa en extern tabell har *inte* ta bort data, endast tabelldefinitionen.

    > [!IMPORTANT]
    > Lämna den __databasen__ valet __standard__. Exemplen i det här dokumentet används standarddatabasen som ingår i HDInsight.

2. Starta frågan med de **Execute** nedan kalkylbladet. Knappen blir orange och ändras texten till **stoppa**.

3. När frågan har slutförts måste den **resultat** visar resultatet av åtgärden. Följande är resultatet av frågan:

        sev       cnt
        [ERROR]   3

    Du kan använda den **loggar** att visa loggningsinformation som skapats för jobbet.

   > [!TIP]
   > Hämta eller spara resultaten från den **spara resultaten** listrutan dialogrutan i övre vänstra hörnet i **Frågeprocessresultat** avsnitt.

4. Välj de fyra första raderna i den här frågan och välj sedan **kör**. Observera att det finns inga resultat när jobbet har slutförts. Med hjälp av den **Execute** knappen när delen av frågan markeras körs bara de valda rapporterna. Markeringen är inte i det här fallet den sista instruktionen som hämtar rader från tabellen. Om du väljer bara den raden och använda **kör**, bör du se ett förväntat resultat.

5. Om du vill lägga till ett kalkylblad, använder den **nytt kalkylblad** knappen längst ned i den **frågeredigeraren**. Ange följande HiveQL-instruktioner i det nya kalkylbladet:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
    ```

  Dessa instruktioner utför följande åtgärder:

   * **Skapa tabell om inte finns**: skapar en tabell om det inte redan finns. Eftersom den **externa** nyckelordet används inte, en intern tabell skapas. En intern tabell lagras i datalagret Hive och hanteras helt av Hive. Till skillnad från med externa tabeller kan släppa en intern tabell tar bort de underliggande data.

   * **LAGRAS AS ORC**: lagrar data i optimerad raden kolumner (ORC)-format. ORC är ett mycket optimerad och effektiv format för att lagra data med Hive.

   * **INFOGA ÖVER... Välj**: väljer rader från den **log4jLogs** tabellen som innehåller `[ERROR]`, och infogar data till den **errorLogs** tabell.

Använd den **kör** för att köra den här frågan. Den **resultat** fliken innehåller inte någon information om frågan returnerar noll rader. Status ska visa **lyckades** när frågan har slutförts.

### <a name="visual-explain"></a>Visual förklarar

Om du vill visa en visualisering av frågeplanen, Välj den **Visual förklarar** tabbtangenten kalkylbladet.

Den **Visual förklarar** visning av frågan kan vara lättare att förstå flödet av komplexa frågor. Du kan se en textrepresentation motsvarigheten till den här vyn genom att använda den **förklara** knappen i frågeredigeraren.

### <a name="tez-ui"></a>Tez-Gränssnittet

Om du vill visa Tez UI för frågan, Välj den **Tez** tabbtangenten kalkylbladet.

> [!IMPORTANT]
> Tez används inte för att matcha alla frågor. Du kan lösa många frågor utan att använda Tez. 

Om Tez användes för att matcha frågan, visas dirigeras acykliska diagram (DAG). Om du vill visa DAG för frågor som du har kört tidigare, eller om du vill felsöka Tez-processen, Använd den [Tez visa](../hdinsight-debug-ambari-tez-view.md) i stället.

## <a name="view-job-history"></a>Visa jobbhistorik

Den __jobb__ visar en historik över Hive-frågor.

![Bild av jobbhistoriken](./media/apache-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a>Databastabeller

Du kan använda den __tabeller__ att arbeta med tabeller i en databas för Hive.

![Bild av fliken tabeller](./media/apache-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a>Sparade frågor

Från den **frågan** fliken kan du om du vill spara frågor. När du har sparat en fråga kan du återanvända den från den __sparade frågor__ fliken.

![Bild av fliken sparade frågor](./media/apache-hadoop-use-hive-ambari-view/saved-queries.png)

## <a name="user-defined-functions"></a>Användardefinierade funktioner

Du kan också utöka Hive via användardefinierade funktioner (UDF). Använda en UDF för att implementera funktioner eller som inte är enkelt modelleras i HiveQL.

Deklarera och spara en uppsättning UDF: er med hjälp av den **UDF** fliken överst i Hive-vy. Dessa UDF: er kan användas med den **frågeredigeraren**.

![Bild av UDF-fliken](./media/apache-hadoop-use-hive-ambari-view/user-defined-functions.png)

När du har lagt till en UDF Hive-vy en **Infoga UDF: er** visas knappen längst ned i den **frågeredigeraren**. Om du markerar den här posten visas listrutan för UDF: er som definierats i Hive-vy. Att välja en UDF lägger till HiveQL-instruktioner i din fråga att aktivera UDF-filen.

Till exempel om du har definierat en UDF med följande egenskaper:

* Resursnamnet: myudfs

* Resursens sökväg: /myudfs.jar

* UDF-namn: myawesomeudf

* Klassnamn för UDF: com.myudfs.Awesome

Med hjälp av den **Infoga UDF: er** knappen visar en post med namnet **myudfs**, med en annan listrutan för varje UDF som definierats för den här resursen. I det här fallet är det **myawesomeudf**. Markerar den här posten läggs följande till början av frågan:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

Du kan sedan använda UDF-filen i frågan. Till exempel `SELECT myawesomeudf(name) FROM people;`.

Mer information om hur du använder UDF: er med Hive i HDInsight finns i följande artiklar:

* [Med hjälp av Python med Hive och Pig i HDInsight](python-udf-hdinsight.md)
* [Lägga till en anpassad UDF Hive i HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a>Hive-inställningar

Du kan ändra olika Hive-inställningar, till exempel ändra motorn för körning för registreringsdata till MapReduce Tez (standard).

## <a id="nextsteps"></a>Nästa steg

Allmän information om Hive i HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)
