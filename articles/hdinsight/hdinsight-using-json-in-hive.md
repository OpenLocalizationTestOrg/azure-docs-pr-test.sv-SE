---
title: Analysera och processen JSON-dokument med Hive i HDInsight | Microsoft Docs
description: "Lär dig hur du använder JSON-dokument och analysera dem med Hive i HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: e17794e8-faae-4264-9434-67f61ea78f13
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/26/2017
ms.author: jgao
ms.openlocfilehash: bd136afebeceb0cd9c24cfc5f15601caa80a755e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Bearbeta och analysera JSON-dokument med hjälp av Hive i HDInsight

Lär dig mer om att bearbeta och analysera JSON-filer med hjälp av Hive i HDInsight. Följande JSON-dokumentet används i självstudiekursen:

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

Filen finns på wasb://processjson@hditutorialdata.blob.core.windows.net/. Mer information om hur du använder Azure Blob storage med HDInsight finns [använda HDFS-kompatibla Azure Blob storage med Hadoop i HDInsight](hdinsight-hadoop-use-blob-storage.md). Du kan kopiera filen till standardbehållaren på klustret.

I den här kursen använder du Hive-konsolen.  Anvisningar för att öppna konsolen Hive finns [använda Hive med Hadoop i HDInsight med fjärrskrivbord](hdinsight-hadoop-use-hive-remote-desktop.md).

## <a name="flatten-json-documents"></a>Förenkla JSON-dokument
De metoder som anges i nästa avsnitt kräver JSON-dokument i en enda rad. Därför måste du förenkla JSON-dokument till en sträng. Om JSON-dokumentet redan förenklas du hoppa över det här steget och gå direkt till nästa avsnitt på Analysera JSON-data.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

Rådata JSON-filen finns på  **wasb://processjson@hditutorialdata.blob.core.windows.net/** . Den *StudentsRaw* Hive tabell pekar till rå förenklade JSON-dokumentet.

Den *StudentsOneLine* Hive-tabell lagrar data i HDInsight filsystemet under den */json/studenter/* sökväg.

INSERT-instruktionen fyller tabellen StudentOneLine med Flat JSON-data.

SELECT-instruktionen kan bara returnera en rad.

Här är resultatet av SELECT-instruktionen:

![Förenkling av JSON-dokumentet.][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a>Analysera JSON-dokument i Hive
Hive innehåller tre olika metoder för att köra frågor på JSON-dokument:

* Använd GET\_JSON\_OBJEKTET UDF (användardefinierad funktion)
* Använd JSON_TUPLE UDF
* Använda anpassade SerDe
* skriva du äger UDF med Python eller andra språk. Se [i den här artikeln] [ hdinsight-python] på köra Python koden med Hive.

### <a name="use-the-getjsonobject-udf"></a>Använd GET\_JSON_OBJECT UDF
Hive ger en inbyggda UDF kallas [hämta json-objekt](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), som kan utföra JSON fråga under körning. Den här metoden tar två argument – tabellnamnet och metodnamnet som har förenklad JSON-dokumentet och JSON-fält som ska tolkas. Nu ska vi titta på ett exempel för att se hur den här UDF fungerar.

Hämta det förnamn och efternamn för varje student

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Här är utdata när du kör den här frågan i konsolfönstret.

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

Det finns några begränsningar för get-json_object UDF.

* Eftersom varje fält i frågan kräver reparsing frågan, påverkar prestanda.
* Hämta\_JSON_OBJECT() returnerar strängrepresentation av en matris. Om du vill konvertera denna matris till en Hive-matris, du måste använda reguljära uttryck för att ersätta hakparentes ' [' och ']' och även anropa delning för att få matrisen.

Det är därför Hive-wiki rekommenderar att du använder json_tuple.  

### <a name="use-the-jsontuple-udf"></a>Använd JSON_TUPLE UDF
En annan UDF som tillhandahålls av Hive kallas [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), vilken som presterar bättre än [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Den här metoden tar en uppsättning nycklar och en JSON-sträng, och returnerar en tuppel av värden med hjälp av en funktion. Följande fråga returnerar student-id och klass från JSON-dokumentet:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

Utdata från skriptet i Hive-konsolen:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

JSON\_TUPPEL använder den [lateral visa](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntax i Hive, vilket gör att json\_tuppel att skapa en virtuell tabell genom att använda funktionen UDT för varje rad i den ursprungliga tabellen.  Komplexa JSONs bli för ohanterlig på grund av upprepade användningen av LATERALA VYN. Dessutom kan inte JSON_TUPLE hantera kapslade JSONs.

### <a name="use-custom-serde"></a>Använda anpassade SerDe
SerDe är det bästa valet för parsning av kapslade JSON-dokument, kan du definiera JSON-schema och använda schemat för att tolka dokument. I kursen får du använder någon av de vanligaste SerDe som har utvecklats av [Roberto Congiu](https://github.com/rcongiu).

**Använda anpassade SerDe:**

1. Installera [Java SE Development sats 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). Välj Windows X64-versionen av JDK om du ska använda Windows-distribution av HDInsight
   
   > [!WARNING]
   > JDK 1.8 fungerar inte med den här SerDe.
   > 
   > 
   
    När installationen är klar, lägger du till en ny miljövariabel för användaren:
   
   1. Öppna **visa avancerade systeminställningar** från Windows-skärmen.
   2. Klicka på **miljövariabler**.  
   3. Lägg till en ny **JAVA_HOME** miljövariabeln pekar till **C:\Program Files\Java\jdk1.7.0_55** eller var din JDK har installerats.
      
      ![Konfigurera rätt konfigurationsvärden för JDK][image-hdi-hivejson-jdk]
2. Installera [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)
   
    Lägga till bin-mappen i din sökväg genom att gå till Control Panel--> Redigera systemvariabler för ditt konto miljövariabler. Följande skärmbild visar hur du gör detta.
   
    ![Konfigurera Maven][image-hdi-hivejson-maven]
3. Klona projektet från [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github-platsen. Du kan göra detta genom att klicka på knappen ”Hämta Zip” som visas i följande skärmbild.
   
    ![Kloning av projektet][image-hdi-hivejson-serde]

4: Gå till mappen där du har hämtat det här paketet och typ ”mvn package”. Detta bör skapa nödvändiga jar-filer som du sedan kan kopiera över till klustret.

5: Gå till mappen i rotmappen dit du hämtade paketet. Ladda upp filen json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar till huvudnod i klustret. Jag oftast placera den under mappen hive binära: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin eller liknande.

6: Skriv ”Lägg till jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar” i hive-fråga. Eftersom min om jar i mappen C:\apps\dist\hive-0.13.x\bin kan jag direkt lägga till jar med namn som:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

   ![Lägger till JAR i ditt projekt][image-hdi-hivejson-addjar]

Du är nu redo att använda SerDe för att köra frågor mot JSON-dokumentet.

Följande exempel skapar en tabell med ett definierat schema:

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

Förnamn och efternamn för studenter

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Här är ett resultat från Hive-konsolen.

![SerDe frågan 1][image-hdi-hivejson-serde_query1]

Att beräkna summan av resultat av JSON-dokumentet

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

Den föregående frågan använder [lateral Visa Expandera](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF att expandera matrisen poäng så att de kan summeras.

Här är utdata från Hive-konsolen.

![SerDe fråga 2][image-hdi-hivejson-serde_query2]

Om du vill hitta som ämnen angivna student har fått fler än 80 poäng:

    SELECT  
      jt.StudentClassCollection.ClassId
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as score  where score > 80;

Den föregående frågan returnerar en Hive-matris till skillnad från get\_json\_-objekt som returnerar en sträng.

![SerDe fråga 3][image-hdi-hivejson-serde_query3]

Om du vill skil felaktiga JSON sedan som beskrivs i den [wiki-sida](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) för den här SerDe kan du uppnå som genom att skriva följande kod:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




## <a name="summary"></a>Sammanfattning
Sammanfattningsvis beror typ av JSON-operator i Hive som du väljer på ditt scenario. Om du har ett enkelt JSON-dokument och du bara ha ett fält för att leta upp på – du kan välja att använda Hive UDF get\_json\_objekt. Om du har fler än en nyckel för att leta upp kan du använda json_tuple. Om du har en kapslad dokument, bör du använda JSON-SerDe.

## <a name="next-steps"></a>Nästa steg

Andra relaterade artiklar finns

* [Använda Hive och HiveQL med Hadoop i HDInsight för att analysera ett exempel Apache log4j-fil](hdinsight-use-hive.md)
* [Analysera svarta fördröjning data med hjälp av Hive i HDInsight](hdinsight-analyze-flight-delay-data.md)
* [Analysera Twitter-data med Hive i HDInsight](hdinsight-analyze-twitter-data.md)
* [Kör ett Hadoop-jobb med hjälp av Azure Cosmos DB och HDInsight](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
