---
title: aaaAnalyze och processen JSON-dokument med Hive i HDInsight | Microsoft Docs
description: "Lär dig hur toouse JSON-dokument och analysera dem med Hive i HDInsight."
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
ms.openlocfilehash: b4b20172e8553f91a446615dc52f2ea2ef24cd04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Bearbeta och analysera JSON-dokument med hjälp av Hive i HDInsight

Lär dig hur tooprocess och analysera JSON-filer med hjälp av Hive i HDInsight. hello följande JSON-dokumentet används i självstudiekursen hello:

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

hello-filen finns på wasb://processjson@hditutorialdata.blob.core.windows.net/. Mer information om hur du använder Azure Blob storage med HDInsight finns [använda HDFS-kompatibla Azure Blob storage med Hadoop i HDInsight](hdinsight-hadoop-use-blob-storage.md). Du kan kopiera filen hello toohello standardbehållaren på klustret.

I den här kursen använder du hello Hive-konsolen.  Instruktioner för att öppna hello Hive-konsolen finns i [använda Hive med Hadoop i HDInsight med fjärrskrivbord](hdinsight-hadoop-use-hive-remote-desktop.md).

## <a name="flatten-json-documents"></a>Förenkla JSON-dokument
hello-metoder som anges i nästa avsnitt av hello kräver hello JSON-dokument i en enda rad. Därför måste du förenkla hello JSON-dokument tooa sträng. Om JSON-dokumentet redan förenklas, kan du hoppa över detta steg och fortsätter raka toohello nästa avsnitt analysera JSON-data.

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

hello raw JSON-filen finns på  **wasb://processjson@hditutorialdata.blob.core.windows.net/** . Hej *StudentsRaw* Hive-tabell pekar toohello raw förenklade JSON-dokumentet.

Hej *StudentsOneLine* Hive-tabell lagrar hello data i hello HDInsight standardfilsystem under hello */json/studenter/* sökväg.

hello INSERT-instruktionen fyller hello StudentOneLine tabell med hello förenklas JSON-data.

hello SELECT-instruktionen kan bara returnera en rad.

Här är hello utdata från hello SELECT-instruktion:

![Förenkling av hello JSON-dokumentet.][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a>Analysera JSON-dokument i Hive
Hive innehåller tre olika mekanismer toorun frågor på JSON-dokument:

* Använd hello GET\_JSON\_OBJEKTET UDF (användardefinierad funktion)
* Använd hello JSON_TUPLE UDF
* Använda anpassade SerDe
* skriva du äger UDF med Python eller andra språk. Se [i den här artikeln] [ hdinsight-python] på köra Python koden med Hive.

### <a name="use-hello-getjsonobject-udf"></a>Använd hello GET\_JSON_OBJECT UDF
Hive ger en inbyggda UDF kallas [hämta json-objekt](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), som kan utföra JSON fråga under körning. Den här metoden tar två argument – hello tabellnamnet och metodnamnet som har hello Flat JSON dokumentet och hello JSON fält som behöver toobe parsas. Nu ska vi titta på en exempel-toosee hur detta UDF fungerar.

Hämta hello förnamn och efternamn för varje student

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Här är hello utdata när du kör den här frågan i konsolfönstret.

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

Det finns några begränsningar av hello get-json_object UDF.

* Eftersom varje fält i hello frågan kräver reparsing hello frågan, påverkar hello prestanda.
* Hämta\_JSON_OBJECT() returnerar hello strängrepresentation av en matris. tooconvert denna matris tooa Hive matris har toouse reguljära uttryck tooreplace hello hakparenteserna ' [' och ']' och sedan också anropet delar tooget hello matris.

Det är därför hello Hive wiki rekommenderar att du använder json_tuple.  

### <a name="use-hello-jsontuple-udf"></a>Använd hello JSON_TUPLE UDF
En annan UDF som tillhandahålls av Hive kallas [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), vilken som presterar bättre än [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Den här metoden tar en uppsättning nycklar och en JSON-sträng, och returnerar en tuppel av värden med hjälp av en funktion. hello returnerar följande fråga hello student id och hello klass från hello JSON-dokument:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

hello utdata från skriptet i hello Hive-konsolen:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

JSON\_TUPPEL använder hello [lateral visa](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntax i Hive, vilket gör att json\_tuppel toocreate en virtuell tabell genom att använda hello UDT funktionen tooeach raden i hello ursprungliga tabellen.  Komplexa JSONs blir för ohanterlig på grund av hello upprepad användning av LATERALA VYN. Dessutom kan inte JSON_TUPLE hantera kapslade JSONs.

### <a name="use-custom-serde"></a>Använda anpassade SerDe
SerDe hello bästa valet för parsning av kapslade JSON-dokument, kan du toodefine hello JSON-schema och Använd hello tooparse hello-schemadokument. I kursen får du använder någon av hello mer populära SerDe som har utvecklats av [Roberto Congiu](https://github.com/rcongiu).

**toouse hello anpassade SerDe:**

1. Installera [Java SE Development sats 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). Välj hello Windows X64 version av hello JDK om du ska toobe med hello Windows-distribution av HDInsight
   
   > [!WARNING]
   > JDK 1.8 fungerar inte med den här SerDe.
   > 
   > 
   
    Lägg till en ny miljövariabel för användaren när hello installationen är klar:
   
   1. Öppna **visa avancerade systeminställningar** från Windows hello-skärmen.
   2. Klicka på **miljövariabler**.  
   3. Lägg till en ny **JAVA_HOME** miljövariabeln pekar för**C:\Program Files\Java\jdk1.7.0_55** eller var din JDK har installerats.
      
      ![Konfigurera rätt konfigurationsvärden för JDK][image-hdi-hivejson-jdk]
2. Installera [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)
   
    Lägg till hello bin tooyour mappsökväg genom att gå tooControl panelen--> Redigera hello systemvariabler för ditt konto miljövariabler. hello följande skärmbild visas hur toodo detta.
   
    ![Konfigurera Maven][image-hdi-hivejson-maven]
3. Klona hello projektet från [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github-platsen. Du kan göra detta genom att klicka på ”Hämta Zip” hello-knappen som visas i följande skärmbild hello.
   
    ![Kloningen hello-projekt][image-hdi-hivejson-serde]

4: gå toohello mappen där du har hämtat det här paketet och typ ”mvn package”. Detta bör skapa hello nödvändiga jar-filer som du sedan kan kopiera över toohello klustret.

5: gå toohello målmappen hello rot i mappen dit du hämtade hello-paketet. Överför hello json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar filen toohead-nod i klustret. Jag vanligtvis placera den under hello hive binära mapp: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin eller liknande.

6: Skriv ”Lägg till jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar” hello hive-fråga. Eftersom i min fallet hello jar i hello C:\apps\dist\hive-0.13.x\bin mapp kan jag direkt till hello jar med hello namnet som visas:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

   ![Lägga till JAR tooyour projekt][image-hdi-hivejson-addjar]

Du är nu redo toouse hello SerDe toorun frågor mot hello JSON-dokumentet.

Följande instruktion hello skapar en tabell med ett definierat schema:

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

toolist hello förnamn och efternamn hello student

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Här är hello resultatet från hello Hive-konsolen.

![SerDe frågan 1][image-hdi-hivejson-serde_query1]

toocalculate hello summan av resultat av hello JSON-dokumentet

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

föregående fråga använder hello [lateral Visa Expandera](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF tooexpand hello matris med resultat så att de kan summeras.

Här är hello utdata från hello Hive-konsolen.

![SerDe fråga 2][image-hdi-hivejson-serde_query2]

toofind som ämnen angivna student har fått fler än 80 poäng:

    SELECT  
      jt.StudentClassCollection.ClassId
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as score  where score > 80;

hello föregående fråga returnerar en Hive-matris till skillnad från get\_json\_-objekt som returnerar en sträng.

![SerDe fråga 3][image-hdi-hivejson-serde_query3]

Om du vill tooskil felaktig JSON sedan som förklaras i hello [wiki-sida](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) för den här SerDe kan du uppnå som genom att skriva följande kod hello:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




## <a name="summary"></a>Sammanfattning
Sammanfattningsvis hello typ av JSON-operator i struktur som du väljer beror på ditt scenario. Om du har ett enkelt JSON-dokument och du bara har ett fält toolook på – du kan välja toouse hello Hive UDF get\_json\_objekt. Om du har flera viktiga toolook på, kan du använda json_tuple. Om du har en kapslad dokument, bör du använda hello JSON SerDe.

## <a name="next-steps"></a>Nästa steg

Andra relaterade artiklar finns

* [Använda Hive och HiveQL med Hadoop i HDInsight tooanalyze ett exempel Apache log4j-fil](hdinsight-use-hive.md)
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
