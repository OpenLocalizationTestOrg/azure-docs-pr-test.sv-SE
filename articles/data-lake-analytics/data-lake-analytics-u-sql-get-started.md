---
title: "Kom igång med U-SQL-språket | Microsoft Docs"
description: "Lär dig grunderna om U-SQL-språket."
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
ms.openlocfilehash: 38c4e1b9bd24ef0b8a81f6154620f3f98d3b5ac1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-u-sql"></a><span data-ttu-id="4d3cb-103">Kom igång med U-SQL</span><span class="sxs-lookup"><span data-stu-id="4d3cb-103">Get started with U-SQL</span></span>
<span data-ttu-id="4d3cb-104">U-SQL är ett språk som kombinerar deklarativ SQL med tvingande C# för att du ska bearbeta data i alla skalor.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-104">U-SQL is a language that combines declarative SQL with imperative C# to let you process data at any scale.</span></span> <span data-ttu-id="4d3cb-105">Via skalbara, distribuerade frågan möjligheterna för U-SQL, kan du effektivt analysera data i relationella butiker, till exempel Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-105">Through the scalable, distributed-query capability of U-SQL, you can efficiently analyze data across relational stores such as Azure SQL Database.</span></span> <span data-ttu-id="4d3cb-106">Du kan bearbeta Ostrukturerade data genom att använda schemat vid läsning och lägga till egen kod och UDF: er med U-SQL.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-106">With U-SQL, you can process unstructured data by applying schema on read and inserting custom logic and UDFs.</span></span> <span data-ttu-id="4d3cb-107">Dessutom innehåller U-SQL utökningsbarhet som ger detaljerad kontroll över hur du utför i större skala.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-107">Additionally, U-SQL includes extensibility that gives you fine-grained control over how to execute at scale.</span></span> 

## <a name="learning-resources"></a><span data-ttu-id="4d3cb-108">Utbildningsresurser</span><span class="sxs-lookup"><span data-stu-id="4d3cb-108">Learning resources</span></span>

* <span data-ttu-id="4d3cb-109">Den [U-SQL-kursen](http://aka.ms/usqltutorial) innehåller en guidad genomgång för de flesta av U-SQL-språket.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-109">The [U-SQL Tutorial](http://aka.ms/usqltutorial) provides a guided walkthrough of most of the U-SQL language.</span></span> <span data-ttu-id="4d3cb-110">Det här dokumentet bör läsa för alla utvecklare som vill lära dig U-SQL.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-110">This document is recommended reading for all developers wanting to learn U-SQL.</span></span>
* <span data-ttu-id="4d3cb-111">Detaljerad information om den **U-SQL-syntaxen för anspråksregelspråket**, finns det [U-SQL-Språkreferens](http://go.microsoft.com/fwlink/p/?LinkId=691348).</span><span class="sxs-lookup"><span data-stu-id="4d3cb-111">For detailed information about the **U-SQL language syntax**, see the [U-SQL Language Reference](http://go.microsoft.com/fwlink/p/?LinkId=691348).</span></span>
* <span data-ttu-id="4d3cb-112">Att förstå den **U-SQL-designfilosofin**, bloggposten Visual Studio [introduktion till U-SQL – ett språk som är stort databearbetning enkelt](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span><span class="sxs-lookup"><span data-stu-id="4d3cb-112">To understand the **U-SQL design philosophy**, see the Visual Studio blog post [Introducing U-SQL – A Language that makes Big Data Processing Easy](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4d3cb-113">Krav</span><span class="sxs-lookup"><span data-stu-id="4d3cb-113">Prerequisites</span></span>

<span data-ttu-id="4d3cb-114">Innan du går igenom U-SQL-exemplen i det här dokumentet, läsa och slutföra [Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4d3cb-114">Before you go through the U-SQL samples in this document, read and complete [Tutorial: Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span> <span data-ttu-id="4d3cb-115">Den här kursen beskrivs säkerhetsnivån med hjälp av U-SQL med Azure Data Lake-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-115">That tutorial explains the mechanics of using U-SQL with Azure Data Lake Tools for Visual Studio.</span></span>

## <a name="your-first-u-sql-script"></a><span data-ttu-id="4d3cb-116">Skriv ditt första U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="4d3cb-116">Your first U-SQL script</span></span>

<span data-ttu-id="4d3cb-117">Följande U-SQL-skriptet är enkel och låt oss se många aspekter U-SQL-språket.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-117">The following U-SQL script is simple and lets us explore many aspects the U-SQL language.</span></span>

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
    TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();
```

<span data-ttu-id="4d3cb-118">Det här skriptet har inte några steg för omvandling.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-118">This script doesn't have any transformation steps.</span></span> <span data-ttu-id="4d3cb-119">Det läser från källfilen kallas `SearchLog.tsv`schematizes den och skriver raduppsättningen tillbaka till en fil med namnet SearchLog-första-u-sql.csv.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-119">It reads from the source file called `SearchLog.tsv`, schematizes it, and writes the rowset back into a file called SearchLog-first-u-sql.csv.</span></span>

<span data-ttu-id="4d3cb-120">Observera data frågetecken skriver i den `Duration` fältet.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-120">Notice the question mark next to the data type in the `Duration` field.</span></span> <span data-ttu-id="4d3cb-121">Det innebär att den `Duration` fältet kan vara null.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-121">It means that the `Duration` field could be null.</span></span>

### <a name="key-concepts"></a><span data-ttu-id="4d3cb-122">Viktiga begrepp</span><span class="sxs-lookup"><span data-stu-id="4d3cb-122">Key concepts</span></span>
* <span data-ttu-id="4d3cb-123">**Raduppsättningen variabler**: varje frågeuttryck som producerar en raduppsättning kan tilldelas till en variabel.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-123">**Rowset variables**: Each query expression that produces a rowset can be assigned to a variable.</span></span> <span data-ttu-id="4d3cb-124">U-SQL följer T-SQL variabeln namngivningsmönstret (`@searchlog`, till exempel) i skriptet.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-124">U-SQL follows the T-SQL variable naming pattern (`@searchlog`, for example) in the script.</span></span>
* <span data-ttu-id="4d3cb-125">Den **EXTRAHERA** nyckelordet läser data från en fil och definierar schemat vid läsning.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-125">The **EXTRACT** keyword reads data from a file and defines the schema on read.</span></span> <span data-ttu-id="4d3cb-126">`Extractors.Tsv`är en inbyggd U-SQL-extraktor för fliken värden filer.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-126">`Extractors.Tsv` is a built-in U-SQL extractor for tab-separated-value files.</span></span> <span data-ttu-id="4d3cb-127">Du kan utveckla anpassade juicepressar.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-127">You can develop custom extractors.</span></span>
* <span data-ttu-id="4d3cb-128">Den **utdata** skriver data från en raduppsättning till en fil.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-128">The **OUTPUT** writes data from a rowset to a file.</span></span> <span data-ttu-id="4d3cb-129">`Outputters.Csv()`är en inbyggd U-SQL-outputter att skapa en CSV-värdefil.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-129">`Outputters.Csv()` is a built-in U-SQL outputter to create a comma-separated-value file.</span></span> <span data-ttu-id="4d3cb-130">Du kan utveckla anpassade outputters.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-130">You can develop custom outputters.</span></span>

### <a name="file-paths"></a><span data-ttu-id="4d3cb-131">Sökvägar</span><span class="sxs-lookup"><span data-stu-id="4d3cb-131">File paths</span></span>

<span data-ttu-id="4d3cb-132">EXTRAHERA och utdata uttryck använder sökvägar.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-132">The EXTRACT and OUTPUT statements use file paths.</span></span> <span data-ttu-id="4d3cb-133">Sökvägar kan vara absolut eller relativ:</span><span class="sxs-lookup"><span data-stu-id="4d3cb-133">File paths can be absolute or relative:</span></span>

<span data-ttu-id="4d3cb-134">Följande absoluta sökvägen refererar till en fil i ett Data Lake Store med namnet `mystore`:</span><span class="sxs-lookup"><span data-stu-id="4d3cb-134">This following absolute file path refers to a file in a Data Lake Store named `mystore`:</span></span>

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

<span data-ttu-id="4d3cb-135">Den här följande sökväg börjar med `"/"`.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-135">This following file path starts with `"/"`.</span></span> <span data-ttu-id="4d3cb-136">Den refererar till en fil i Data Lake Store-standardkontot:</span><span class="sxs-lookup"><span data-stu-id="4d3cb-136">It refers to a file in the default Data Lake Store account:</span></span>

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a><span data-ttu-id="4d3cb-137">Använd skalbara variabler</span><span class="sxs-lookup"><span data-stu-id="4d3cb-137">Use scalar variables</span></span>

<span data-ttu-id="4d3cb-138">Du kan använda skalbara variabler samt för att underlätta skript-underhåll.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-138">You can use scalar variables as well to make your script maintenance easier.</span></span> <span data-ttu-id="4d3cb-139">Tidigare U-SQL-skriptet kan också anges som:</span><span class="sxs-lookup"><span data-stu-id="4d3cb-139">The previous U-SQL script can also be written as:</span></span>

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
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a><span data-ttu-id="4d3cb-140">Transformera raduppsättningar</span><span class="sxs-lookup"><span data-stu-id="4d3cb-140">Transform rowsets</span></span>

<span data-ttu-id="4d3cb-141">Använd **Välj** att omvandla raduppsättningar:</span><span class="sxs-lookup"><span data-stu-id="4d3cb-141">Use **SELECT** to transform rowsets:</span></span>

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
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

<span data-ttu-id="4d3cb-142">WHERE-satsen använder en [C# booleskt uttryck](https://msdn.microsoft.com/library/6a71f45d.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d3cb-142">The WHERE clause uses a [C# Boolean expression](https://msdn.microsoft.com/library/6a71f45d.aspx).</span></span> <span data-ttu-id="4d3cb-143">Du kan använda uttryck-språket C# för att göra egna uttryck och funktioner.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-143">You can use the C# expression language to do your own expressions and functions.</span></span> <span data-ttu-id="4d3cb-144">Du kan även utföra mer komplexa filtrering genom att kombinera dem med logiska konjunktioner (och) och disjunctions (ORs).</span><span class="sxs-lookup"><span data-stu-id="4d3cb-144">You can even perform more complex filtering by combining them with logical conjunctions (ANDs) and disjunctions (ORs).</span></span>

<span data-ttu-id="4d3cb-145">Följande skript använder metoden DateTime.Parse() och en tillsammans.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-145">The following script uses the DateTime.Parse() method and a conjunction.</span></span>

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
        TO "/output/SearchLog-transform-datetime.csv"
        USING Outputters.Csv();

 >[!NOTE]
 ><span data-ttu-id="4d3cb-146">Den andra frågan körs på resultatet av raduppsättningen första som skapar en kombination av två filter.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-146">The second query is operating on the result of the first rowset, which creates a composite of the two filters.</span></span> <span data-ttu-id="4d3cb-147">Du kan också använda ett variabelnamn och namnen är begränsade lexically.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-147">You can also reuse a variable name, and the names are scoped lexically.</span></span>

## <a name="aggregate-rowsets"></a><span data-ttu-id="4d3cb-148">Sammanställd raduppsättningar</span><span class="sxs-lookup"><span data-stu-id="4d3cb-148">Aggregate rowsets</span></span>
<span data-ttu-id="4d3cb-149">U-SQL ger dig den välkända ORDER BY, GROUP BY och aggregeringar.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-149">U-SQL gives you the familiar ORDER BY, GROUP BY, and aggregations.</span></span>

<span data-ttu-id="4d3cb-150">Följande fråga hittar den totala varaktigheten per region och visar sedan upp fem varaktighet i ordning.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-150">The following query finds the total duration per region, and then displays the top five durations in order.</span></span>

<span data-ttu-id="4d3cb-151">U-SQL-raduppsättningar bevaras inte deras inbördes ordning för nästa fråga.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-151">U-SQL rowsets do not preserve their order for the next query.</span></span> <span data-ttu-id="4d3cb-152">Därför om du vill ordna utdata, måste du lägga till ORDER BY i utdata-instruktion:</span><span class="sxs-lookup"><span data-stu-id="4d3cb-152">Thus, to order an output, you need to add ORDER BY to the OUTPUT statement:</span></span>

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
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

<span data-ttu-id="4d3cb-153">U-SQL ORDER BY-satsen kräver att du använder FETCH-sats i en SELECT-uttryck.</span><span class="sxs-lookup"><span data-stu-id="4d3cb-153">The U-SQL ORDER BY clause requires using the FETCH clause in a SELECT expression.</span></span>

<span data-ttu-id="4d3cb-154">MED U-SQL-instruktion kan användas för att begränsa resultatet till grupper som uppfyller villkoret HAVING:</span><span class="sxs-lookup"><span data-stu-id="4d3cb-154">The U-SQL HAVING clause can be used to restrict the output to groups that satisfy the HAVING condition:</span></span>

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
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

<span data-ttu-id="4d3cb-155">Avancerade aggregering scenarier finns i referensdokumentationen för U-SQL-för [sammanställa, analytiska och referera till funktioner](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)</span><span class="sxs-lookup"><span data-stu-id="4d3cb-155">For advanced aggregation scenarios, see the The U-SQL reference documentation for [aggregate, analytic, and reference functions](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d3cb-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4d3cb-156">Next steps</span></span>
* [<span data-ttu-id="4d3cb-157">Översikt över Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="4d3cb-157">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="4d3cb-158">Utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4d3cb-158">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
