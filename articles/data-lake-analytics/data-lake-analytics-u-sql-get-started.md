---
title: "Kom igång med U-SQL-språket | Microsoft Docs"
description: "Lär dig grunderna hello av hello U-SQL-språket."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/23/2017
ms.author: saveenr
ms.openlocfilehash: 5edee0e0d85211e84b3d47895c53d71f0a19f083
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-u-sql"></a>Kom igång med U-SQL
U-SQL är ett språk som kombinerar deklarativ SQL med tvingande C# toolet du bearbetar data i alla skalor. Via hello skalbara, distribuerade frågan möjligheterna för U-SQL, kan du effektivt analysera data i relationella butiker, till exempel Azure SQL Database. Du kan bearbeta Ostrukturerade data genom att använda schemat vid läsning och lägga till egen kod och UDF: er med U-SQL. Dessutom innehåller U-SQL utökningsbarhet som ger detaljerad kontroll över hur tooexecute i större skala. 

## <a name="learning-resources"></a>Utbildningsresurser

* Hej [U-SQL-kursen](http://aka.ms/usqltutorial) innehåller en guidad genomgång för de flesta hello U-SQL-språket. Det här dokumentet bör läsa för alla utvecklare som toolearn U-SQL.
* Detaljerad information om hello **U-SQL-syntaxen för anspråksregelspråket**, se hello [U-SQL-Språkreferens](http://go.microsoft.com/fwlink/p/?LinkId=691348).
* toounderstand hello **U-SQL-designfilosofin**, finns hello Visual Studio bloggposten [introduktion till U-SQL – ett språk som är stort databearbetning enkelt](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

## <a name="prerequisites"></a>Krav

Innan du går igenom hello U-SQL-exempel i det här dokumentet, läsa och slutföra [Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md). Den här kursen beskrivs hello säkerhetsnivån med hjälp av U-SQL med Azure Data Lake-verktyg för Visual Studio.

## <a name="your-first-u-sql-script"></a>Skriv ditt första U-SQL-skript

hello följande U-SQL-skriptet är enkel och låt oss se många aspekter hello U-SQL-språket.

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog   
    too"/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();
```

Det här skriptet har inte några steg för omvandling. Det läser från hello källfilen kallas `SearchLog.tsv`schematizes den och skriver hello raduppsättningen tillbaka till en fil med namnet SearchLog-första-u-sql.csv.

Lägg märke till hello frågetecken nästa toohello datatyp i hello `Duration` fältet. Det innebär att hello `Duration` fältet kan vara null.

### <a name="key-concepts"></a>Viktiga begrepp
* **Raduppsättningen variabler**: varje frågeuttryck som producerar en raduppsättning kan tilldelas tooa variabeln. U-SQL följer hello T-SQL mönstret för variabeln (`@searchlog`, till exempel) i hello skript.
* Hej **EXTRAHERA** nyckelordet läser data från en fil och definierar hello schema vid läsning. `Extractors.Tsv`är en inbyggd U-SQL-extraktor för fliken värden filer. Du kan utveckla anpassade juicepressar.
* Hej **utdata** skriver data från en raduppsättning tooa-fil. `Outputters.Csv()`är en inbyggd U-SQL outputter toocreate en CSV-värdefil. Du kan utveckla anpassade outputters.

### <a name="file-paths"></a>Sökvägar

använda sökvägar hello EXTRAHERA och utdata-instruktioner. Sökvägar kan vara absolut eller relativ:

Följande absoluta sökvägen refererar tooa fil i ett Data Lake Store med namnet `mystore`:

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

Den här följande sökväg börjar med `"/"`. Den hänvisar tooa filen i Data Lake Store-standardkontot hello:

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a>Använd skalbara variabler

Du kan använda skalbara variabler som korrekt toomake skript-Underhåll enklare. hello tidigare U-SQL-skript kan också anges som:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        too@out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Transformera raduppsättningar

Använd **Välj** tootransform raduppsättningar:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        too"/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

Hej WHERE-satsen använder en [C# booleskt uttryck](https://msdn.microsoft.com/library/6a71f45d.aspx). Du kan använda hello C# uttryck språk toodo egna uttryck och funktioner. Du kan även utföra mer komplexa filtrering genom att kombinera dem med logiska konjunktioner (och) och disjunctions (ORs).

hello använder följande skript hello DateTime.Parse() metod och en tillsammans.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        too"/output/SearchLog-transform-datetime.csv"
        USING Outputters.Csv();

 >[!NOTE]
 >hello andra frågan körs på hello resultatet av hello första raduppsättningen, vilket skapar en kombination av hello två filter. Du kan också använda ett variabelnamn och hello namn är begränsade lexically.

## <a name="aggregate-rowsets"></a>Sammanställd raduppsättningar
U-SQL ger du hello bekant ORDER BY, GROUP BY och aggregeringar.

hello följande fråga hittar hello totala varaktigheten per region och sedan visar hello upp fem varaktighet i ordning.

U-SQL-raduppsättningar bevaras inte deras inbördes ordning för hello nästa fråga. Därför tooorder en utdata som du behöver tooadd ORDER BY toohello utdata instruktionen:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @rs1
        too@out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

    OUTPUT @res
        too@out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

hello-U-SQL ORDER BY-satsen kräver att du använder hello FETCH-sats i en SELECT-uttryck.

hello med U-SQL-satsen kan vara används toorestrict hello utdata toogroups som uppfyller hello HAVING villkor:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
        GROUP BY Region
        HAVING SUM(Duration) > 200;

    OUTPUT @res
        too"/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Avancerade aggregering scenarier finns hello hello U-SQL-referensdokumentationen för [sammanställa, analytiska och referera till funktioner](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)

## <a name="next-steps"></a>Nästa steg
* [Översikt över Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
