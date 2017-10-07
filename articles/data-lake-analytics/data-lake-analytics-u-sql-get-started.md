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
# <a name="get-started-with-u-sql"></a><span data-ttu-id="f28d5-103">Kom igång med U-SQL</span><span class="sxs-lookup"><span data-stu-id="f28d5-103">Get started with U-SQL</span></span>
<span data-ttu-id="f28d5-104">U-SQL är ett språk som kombinerar deklarativ SQL med tvingande C# toolet du bearbetar data i alla skalor.</span><span class="sxs-lookup"><span data-stu-id="f28d5-104">U-SQL is a language that combines declarative SQL with imperative C# toolet you process data at any scale.</span></span> <span data-ttu-id="f28d5-105">Via hello skalbara, distribuerade frågan möjligheterna för U-SQL, kan du effektivt analysera data i relationella butiker, till exempel Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="f28d5-105">Through hello scalable, distributed-query capability of U-SQL, you can efficiently analyze data across relational stores such as Azure SQL Database.</span></span> <span data-ttu-id="f28d5-106">Du kan bearbeta Ostrukturerade data genom att använda schemat vid läsning och lägga till egen kod och UDF: er med U-SQL.</span><span class="sxs-lookup"><span data-stu-id="f28d5-106">With U-SQL, you can process unstructured data by applying schema on read and inserting custom logic and UDFs.</span></span> <span data-ttu-id="f28d5-107">Dessutom innehåller U-SQL utökningsbarhet som ger detaljerad kontroll över hur tooexecute i större skala.</span><span class="sxs-lookup"><span data-stu-id="f28d5-107">Additionally, U-SQL includes extensibility that gives you fine-grained control over how tooexecute at scale.</span></span> 

## <a name="learning-resources"></a><span data-ttu-id="f28d5-108">Utbildningsresurser</span><span class="sxs-lookup"><span data-stu-id="f28d5-108">Learning resources</span></span>

* <span data-ttu-id="f28d5-109">Hej [U-SQL-kursen](http://aka.ms/usqltutorial) innehåller en guidad genomgång för de flesta hello U-SQL-språket.</span><span class="sxs-lookup"><span data-stu-id="f28d5-109">hello [U-SQL Tutorial](http://aka.ms/usqltutorial) provides a guided walkthrough of most of hello U-SQL language.</span></span> <span data-ttu-id="f28d5-110">Det här dokumentet bör läsa för alla utvecklare som toolearn U-SQL.</span><span class="sxs-lookup"><span data-stu-id="f28d5-110">This document is recommended reading for all developers wanting toolearn U-SQL.</span></span>
* <span data-ttu-id="f28d5-111">Detaljerad information om hello **U-SQL-syntaxen för anspråksregelspråket**, se hello [U-SQL-Språkreferens](http://go.microsoft.com/fwlink/p/?LinkId=691348).</span><span class="sxs-lookup"><span data-stu-id="f28d5-111">For detailed information about hello **U-SQL language syntax**, see hello [U-SQL Language Reference](http://go.microsoft.com/fwlink/p/?LinkId=691348).</span></span>
* <span data-ttu-id="f28d5-112">toounderstand hello **U-SQL-designfilosofin**, finns hello Visual Studio bloggposten [introduktion till U-SQL – ett språk som är stort databearbetning enkelt](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span><span class="sxs-lookup"><span data-stu-id="f28d5-112">toounderstand hello **U-SQL design philosophy**, see hello Visual Studio blog post [Introducing U-SQL – A Language that makes Big Data Processing Easy](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f28d5-113">Krav</span><span class="sxs-lookup"><span data-stu-id="f28d5-113">Prerequisites</span></span>

<span data-ttu-id="f28d5-114">Innan du går igenom hello U-SQL-exempel i det här dokumentet, läsa och slutföra [Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f28d5-114">Before you go through hello U-SQL samples in this document, read and complete [Tutorial: Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span> <span data-ttu-id="f28d5-115">Den här kursen beskrivs hello säkerhetsnivån med hjälp av U-SQL med Azure Data Lake-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f28d5-115">That tutorial explains hello mechanics of using U-SQL with Azure Data Lake Tools for Visual Studio.</span></span>

## <a name="your-first-u-sql-script"></a><span data-ttu-id="f28d5-116">Skriv ditt första U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="f28d5-116">Your first U-SQL script</span></span>

<span data-ttu-id="f28d5-117">hello följande U-SQL-skriptet är enkel och låt oss se många aspekter hello U-SQL-språket.</span><span class="sxs-lookup"><span data-stu-id="f28d5-117">hello following U-SQL script is simple and lets us explore many aspects hello U-SQL language.</span></span>

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

<span data-ttu-id="f28d5-118">Det här skriptet har inte några steg för omvandling.</span><span class="sxs-lookup"><span data-stu-id="f28d5-118">This script doesn't have any transformation steps.</span></span> <span data-ttu-id="f28d5-119">Det läser från hello källfilen kallas `SearchLog.tsv`schematizes den och skriver hello raduppsättningen tillbaka till en fil med namnet SearchLog-första-u-sql.csv.</span><span class="sxs-lookup"><span data-stu-id="f28d5-119">It reads from hello source file called `SearchLog.tsv`, schematizes it, and writes hello rowset back into a file called SearchLog-first-u-sql.csv.</span></span>

<span data-ttu-id="f28d5-120">Lägg märke till hello frågetecken nästa toohello datatyp i hello `Duration` fältet.</span><span class="sxs-lookup"><span data-stu-id="f28d5-120">Notice hello question mark next toohello data type in hello `Duration` field.</span></span> <span data-ttu-id="f28d5-121">Det innebär att hello `Duration` fältet kan vara null.</span><span class="sxs-lookup"><span data-stu-id="f28d5-121">It means that hello `Duration` field could be null.</span></span>

### <a name="key-concepts"></a><span data-ttu-id="f28d5-122">Viktiga begrepp</span><span class="sxs-lookup"><span data-stu-id="f28d5-122">Key concepts</span></span>
* <span data-ttu-id="f28d5-123">**Raduppsättningen variabler**: varje frågeuttryck som producerar en raduppsättning kan tilldelas tooa variabeln.</span><span class="sxs-lookup"><span data-stu-id="f28d5-123">**Rowset variables**: Each query expression that produces a rowset can be assigned tooa variable.</span></span> <span data-ttu-id="f28d5-124">U-SQL följer hello T-SQL mönstret för variabeln (`@searchlog`, till exempel) i hello skript.</span><span class="sxs-lookup"><span data-stu-id="f28d5-124">U-SQL follows hello T-SQL variable naming pattern (`@searchlog`, for example) in hello script.</span></span>
* <span data-ttu-id="f28d5-125">Hej **EXTRAHERA** nyckelordet läser data från en fil och definierar hello schema vid läsning.</span><span class="sxs-lookup"><span data-stu-id="f28d5-125">hello **EXTRACT** keyword reads data from a file and defines hello schema on read.</span></span> <span data-ttu-id="f28d5-126">`Extractors.Tsv`är en inbyggd U-SQL-extraktor för fliken värden filer.</span><span class="sxs-lookup"><span data-stu-id="f28d5-126">`Extractors.Tsv` is a built-in U-SQL extractor for tab-separated-value files.</span></span> <span data-ttu-id="f28d5-127">Du kan utveckla anpassade juicepressar.</span><span class="sxs-lookup"><span data-stu-id="f28d5-127">You can develop custom extractors.</span></span>
* <span data-ttu-id="f28d5-128">Hej **utdata** skriver data från en raduppsättning tooa-fil.</span><span class="sxs-lookup"><span data-stu-id="f28d5-128">hello **OUTPUT** writes data from a rowset tooa file.</span></span> <span data-ttu-id="f28d5-129">`Outputters.Csv()`är en inbyggd U-SQL outputter toocreate en CSV-värdefil.</span><span class="sxs-lookup"><span data-stu-id="f28d5-129">`Outputters.Csv()` is a built-in U-SQL outputter toocreate a comma-separated-value file.</span></span> <span data-ttu-id="f28d5-130">Du kan utveckla anpassade outputters.</span><span class="sxs-lookup"><span data-stu-id="f28d5-130">You can develop custom outputters.</span></span>

### <a name="file-paths"></a><span data-ttu-id="f28d5-131">Sökvägar</span><span class="sxs-lookup"><span data-stu-id="f28d5-131">File paths</span></span>

<span data-ttu-id="f28d5-132">använda sökvägar hello EXTRAHERA och utdata-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="f28d5-132">hello EXTRACT and OUTPUT statements use file paths.</span></span> <span data-ttu-id="f28d5-133">Sökvägar kan vara absolut eller relativ:</span><span class="sxs-lookup"><span data-stu-id="f28d5-133">File paths can be absolute or relative:</span></span>

<span data-ttu-id="f28d5-134">Följande absoluta sökvägen refererar tooa fil i ett Data Lake Store med namnet `mystore`:</span><span class="sxs-lookup"><span data-stu-id="f28d5-134">This following absolute file path refers tooa file in a Data Lake Store named `mystore`:</span></span>

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

<span data-ttu-id="f28d5-135">Den här följande sökväg börjar med `"/"`.</span><span class="sxs-lookup"><span data-stu-id="f28d5-135">This following file path starts with `"/"`.</span></span> <span data-ttu-id="f28d5-136">Den hänvisar tooa filen i Data Lake Store-standardkontot hello:</span><span class="sxs-lookup"><span data-stu-id="f28d5-136">It refers tooa file in hello default Data Lake Store account:</span></span>

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a><span data-ttu-id="f28d5-137">Använd skalbara variabler</span><span class="sxs-lookup"><span data-stu-id="f28d5-137">Use scalar variables</span></span>

<span data-ttu-id="f28d5-138">Du kan använda skalbara variabler som korrekt toomake skript-Underhåll enklare.</span><span class="sxs-lookup"><span data-stu-id="f28d5-138">You can use scalar variables as well toomake your script maintenance easier.</span></span> <span data-ttu-id="f28d5-139">hello tidigare U-SQL-skript kan också anges som:</span><span class="sxs-lookup"><span data-stu-id="f28d5-139">hello previous U-SQL script can also be written as:</span></span>

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

## <a name="transform-rowsets"></a><span data-ttu-id="f28d5-140">Transformera raduppsättningar</span><span class="sxs-lookup"><span data-stu-id="f28d5-140">Transform rowsets</span></span>

<span data-ttu-id="f28d5-141">Använd **Välj** tootransform raduppsättningar:</span><span class="sxs-lookup"><span data-stu-id="f28d5-141">Use **SELECT** tootransform rowsets:</span></span>

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

<span data-ttu-id="f28d5-142">Hej WHERE-satsen använder en [C# booleskt uttryck](https://msdn.microsoft.com/library/6a71f45d.aspx).</span><span class="sxs-lookup"><span data-stu-id="f28d5-142">hello WHERE clause uses a [C# Boolean expression](https://msdn.microsoft.com/library/6a71f45d.aspx).</span></span> <span data-ttu-id="f28d5-143">Du kan använda hello C# uttryck språk toodo egna uttryck och funktioner.</span><span class="sxs-lookup"><span data-stu-id="f28d5-143">You can use hello C# expression language toodo your own expressions and functions.</span></span> <span data-ttu-id="f28d5-144">Du kan även utföra mer komplexa filtrering genom att kombinera dem med logiska konjunktioner (och) och disjunctions (ORs).</span><span class="sxs-lookup"><span data-stu-id="f28d5-144">You can even perform more complex filtering by combining them with logical conjunctions (ANDs) and disjunctions (ORs).</span></span>

<span data-ttu-id="f28d5-145">hello använder följande skript hello DateTime.Parse() metod och en tillsammans.</span><span class="sxs-lookup"><span data-stu-id="f28d5-145">hello following script uses hello DateTime.Parse() method and a conjunction.</span></span>

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
 ><span data-ttu-id="f28d5-146">hello andra frågan körs på hello resultatet av hello första raduppsättningen, vilket skapar en kombination av hello två filter.</span><span class="sxs-lookup"><span data-stu-id="f28d5-146">hello second query is operating on hello result of hello first rowset, which creates a composite of hello two filters.</span></span> <span data-ttu-id="f28d5-147">Du kan också använda ett variabelnamn och hello namn är begränsade lexically.</span><span class="sxs-lookup"><span data-stu-id="f28d5-147">You can also reuse a variable name, and hello names are scoped lexically.</span></span>

## <a name="aggregate-rowsets"></a><span data-ttu-id="f28d5-148">Sammanställd raduppsättningar</span><span class="sxs-lookup"><span data-stu-id="f28d5-148">Aggregate rowsets</span></span>
<span data-ttu-id="f28d5-149">U-SQL ger du hello bekant ORDER BY, GROUP BY och aggregeringar.</span><span class="sxs-lookup"><span data-stu-id="f28d5-149">U-SQL gives you hello familiar ORDER BY, GROUP BY, and aggregations.</span></span>

<span data-ttu-id="f28d5-150">hello följande fråga hittar hello totala varaktigheten per region och sedan visar hello upp fem varaktighet i ordning.</span><span class="sxs-lookup"><span data-stu-id="f28d5-150">hello following query finds hello total duration per region, and then displays hello top five durations in order.</span></span>

<span data-ttu-id="f28d5-151">U-SQL-raduppsättningar bevaras inte deras inbördes ordning för hello nästa fråga.</span><span class="sxs-lookup"><span data-stu-id="f28d5-151">U-SQL rowsets do not preserve their order for hello next query.</span></span> <span data-ttu-id="f28d5-152">Därför tooorder en utdata som du behöver tooadd ORDER BY toohello utdata instruktionen:</span><span class="sxs-lookup"><span data-stu-id="f28d5-152">Thus, tooorder an output, you need tooadd ORDER BY toohello OUTPUT statement:</span></span>

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

<span data-ttu-id="f28d5-153">hello-U-SQL ORDER BY-satsen kräver att du använder hello FETCH-sats i en SELECT-uttryck.</span><span class="sxs-lookup"><span data-stu-id="f28d5-153">hello U-SQL ORDER BY clause requires using hello FETCH clause in a SELECT expression.</span></span>

<span data-ttu-id="f28d5-154">hello med U-SQL-satsen kan vara används toorestrict hello utdata toogroups som uppfyller hello HAVING villkor:</span><span class="sxs-lookup"><span data-stu-id="f28d5-154">hello U-SQL HAVING clause can be used toorestrict hello output toogroups that satisfy hello HAVING condition:</span></span>

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

<span data-ttu-id="f28d5-155">Avancerade aggregering scenarier finns hello hello U-SQL-referensdokumentationen för [sammanställa, analytiska och referera till funktioner](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)</span><span class="sxs-lookup"><span data-stu-id="f28d5-155">For advanced aggregation scenarios, see hello hello U-SQL reference documentation for [aggregate, analytic, and reference functions](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f28d5-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f28d5-156">Next steps</span></span>
* [<span data-ttu-id="f28d5-157">Översikt över Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="f28d5-157">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="f28d5-158">Utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f28d5-158">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
