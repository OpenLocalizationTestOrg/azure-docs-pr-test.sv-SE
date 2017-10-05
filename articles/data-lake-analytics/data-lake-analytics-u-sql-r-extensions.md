---
title: "Utöka U-SQL-skript med R i Azure Data Lake Analytics | Microsoft Docs"
description: "Lär dig hur du kör R-koden i U-SQL-skript"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: sukvg
editor: cgronlun
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/20/2017
ms.author: saveenr
ms.openlocfilehash: d479af515566f497d9611e75426f6acb8f8276d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-r"></a><span data-ttu-id="9bfbf-103">Självstudier: Kom igång med att utöka U-SQL med R</span><span class="sxs-lookup"><span data-stu-id="9bfbf-103">Tutorial: Get started with extending U-SQL with R</span></span>

<span data-ttu-id="9bfbf-104">I följande exempel visas de grundläggande stegen för att distribuera R-koden:</span><span class="sxs-lookup"><span data-stu-id="9bfbf-104">The following example illustrates the basic steps for deploying R code:</span></span>
* <span data-ttu-id="9bfbf-105">Använd den `REFERENCE ASSEMBLY` -instruktionen för att aktivera R-tillägg för U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-105">Use the `REFERENCE ASSEMBLY` statement to enable R extensions for the U-SQL Script.</span></span>
* <span data-ttu-id="9bfbf-106">Använd den` REDUCE` åtgärden att partitionera indata för en nyckel.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-106">Use the` REDUCE` operation to partition the input data on a key.</span></span>
* <span data-ttu-id="9bfbf-107">R-tillägg för U-SQL är en inbyggd reducer (`Extension.R.Reducer`) R-koden körs på varje nod som tilldelats reducer.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-107">The R extensions for U-SQL include a built-in reducer (`Extension.R.Reducer`) that runs R code on each vertex assigned to the reducer.</span></span> 
* <span data-ttu-id="9bfbf-108">Användning av dedikerade med namnet ramar kallas `inputFromUSQL` och `outputToUSQL `respektive att skicka data mellan U-SQL och R. indata och utdata DataFrame identifierarnamn korrigeras (det vill säga användare kan inte ändra dessa fördefinierade namn på indata och utdata DataFrame identifierare).</span><span class="sxs-lookup"><span data-stu-id="9bfbf-108">Usage of dedicated named data frames called `inputFromUSQL` and `outputToUSQL `respectively to pass data between U-SQL and R. Input and output DataFrame identifier names are fixed (that is, users cannot change these predefined names of input and output DataFrame identifiers).</span></span>

## <a name="embedding-r-code-in-the-u-sql-script"></a><span data-ttu-id="9bfbf-109">Bädda in R-koden i U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="9bfbf-109">Embedding R code in the U-SQL script</span></span>

<span data-ttu-id="9bfbf-110">Du kan infogade R code U-SQL-skript med hjälp av Kommandoparametern för den `Extension.R.Reducer`.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-110">You can inline the R code your U-SQL script by using the command parameter of the `Extension.R.Reducer`.</span></span> <span data-ttu-id="9bfbf-111">Du kan exempelvis deklarera R-skriptet som en string-variabel och skickar den som en parameter till Reducer.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-111">For example, you can declare the R script as a string variable and pass it as a parameter to the Reducer.</span></span>


    REFERENCE ASSEMBLY [ExtR];
    
    DECLARE @myRScript = @"
    inputFromUSQL$Species = as.factor(inputFromUSQL$Species)
    lm.fit=lm(unclass(Species)~.-Par, data=inputFromUSQL)
    #do not return readonly columns and make sure that the column names are the same in usql and r scripts,
    outputToUSQL=data.frame(summary(lm.fit)$coefficients)
    colnames(outputToUSQL) <- c(""Estimate"", ""StdError"", ""tValue"", ""Pr"")
    outputToUSQL
    ";
    
    @RScriptOutput = REDUCE … USING new Extension.R.Reducer(command:@myRScript, rReturnType:"dataframe");

## <a name="keep-the-r-code-in-a-separate-file-and-reference-it--the-u-sql-script"></a><span data-ttu-id="9bfbf-112">Behåll R-koden i en separat fil och hänvisar det U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="9bfbf-112">Keep the R code in a separate file and reference it  the U-SQL script</span></span>

<span data-ttu-id="9bfbf-113">Följande exempel illustrerar användning av en mer komplex.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-113">The following example illustrates a more complex usage.</span></span> <span data-ttu-id="9bfbf-114">I det här fallet distribueras R-koden som en resurs som U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-114">In this case, the R code is deployed as a RESOURCE that is the U-SQL script.</span></span>

<span data-ttu-id="9bfbf-115">Spara den här R-koden som en separat fil.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-115">Save this R code as a separate file.</span></span>

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

<span data-ttu-id="9bfbf-116">Använd ett U-SQL-skript för att distribuera det R-skriptet med instruktionen DISTRIBUERA resurs.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-116">Use a U-SQL script to deploy that R script with the DEPLOY RESOURCE statement.</span></span>

    REFERENCE ASSEMBLY [ExtR];

    DEPLOY RESOURCE @"/usqlext/samples/R/RinUSQL_PredictUsingLinearModelasDF.R";
    DEPLOY RESOURCE @"/usqlext/samples/R/my_model_LM_Iris.rda";
    DECLARE @IrisData string = @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFilePredictions string = @"/my/R/Output/LMPredictionsIris.txt";
    DECLARE @PartitionCount int = 10;

    @InputData =
        EXTRACT 
            SepalLength double,
            SepalWidth double,
            PetalLength double,
            PetalWidth double,
            Species string
        FROM @IrisData
        USING Extractors.Csv();

    @ExtendedData =
        SELECT 
            Extension.R.RandomNumberGenerator.GetRandomNumber(@PartitionCount) AS Par,
            SepalLength,
            SepalWidth,
            PetalLength,
            PetalWidth
        FROM @InputData;

    // Predict Species

    @RScriptOutput = REDUCE @ExtendedData ON Par
        PRODUCE Par, fit double, lwr double, upr double
        READONLY Par
        USING new Extension.R.Reducer(scriptFile:"RinUSQL_PredictUsingLinearModelasDF.R", rReturnType:"dataframe", stringsAsFactors:false);
        OUTPUT @RScriptOutput TO @OutputFilePredictions USING Outputters.Tsv();

## <a name="how-r-integrates-with-u-sql"></a><span data-ttu-id="9bfbf-117">Hur R kan integreras med U-SQL</span><span class="sxs-lookup"><span data-stu-id="9bfbf-117">How R Integrates with U-SQL</span></span>

### <a name="datatypes"></a><span data-ttu-id="9bfbf-118">Datatyper</span><span class="sxs-lookup"><span data-stu-id="9bfbf-118">Datatypes</span></span>
* <span data-ttu-id="9bfbf-119">Konverteras som sträng och numeriska kolumner från U-SQL-mellan R DataFrame och U-SQL [typer som stöds: `double`, `string`, `bool`, `integer`, `byte`].</span><span class="sxs-lookup"><span data-stu-id="9bfbf-119">String and numeric columns from U-SQL are converted as-is between R DataFrame and U-SQL [supported types: `double`, `string`, `bool`, `integer`, `byte`].</span></span>
* <span data-ttu-id="9bfbf-120">Den `Factor` stöds inte i U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-120">The `Factor` datatype is not supported in U-SQL.</span></span>
* <span data-ttu-id="9bfbf-121">`byte[]`måste serialiseras som en base64-kodad `string`.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-121">`byte[]` must be serialized as a base64-encoded `string`.</span></span>
* <span data-ttu-id="9bfbf-122">U-SQL-strängar kan konverteras till faktorer i R-koden när U-SQL skapar R inkommande dataframe eller genom att ange parametern reducer `stringsAsFactors: true`.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-122">U-SQL strings can be converted to factors in R code, once U-SQL create R input dataframe or by setting the reducer parameter `stringsAsFactors: true`.</span></span>

### <a name="schemas"></a><span data-ttu-id="9bfbf-123">Scheman</span><span class="sxs-lookup"><span data-stu-id="9bfbf-123">Schemas</span></span>
* <span data-ttu-id="9bfbf-124">U-SQL-datauppsättningar kan inte ha dubbletter av kolumnnamn.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-124">U-SQL datasets cannot have duplicate column names.</span></span>
* <span data-ttu-id="9bfbf-125">U-SQL datauppsättningar kolumnnamn måste vara strängar.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-125">U-SQL datasets column names must be strings.</span></span>
* <span data-ttu-id="9bfbf-126">Kolumnnamnen måste vara samma i U-SQL och R-skript.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-126">Column names must be the same in U-SQL and R scripts.</span></span>
* <span data-ttu-id="9bfbf-127">ReadOnly-kolumn kan inte ingå i utdata dataframe.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-127">Readonly column cannot be part of the output dataframe.</span></span> <span data-ttu-id="9bfbf-128">Eftersom readonly kolumner automatiskt matas in tillbaka i U-SQL-tabell om det är en del av utdataschemat för UDO.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-128">Because readonly columns are automatically injected back in the U-SQL table if it is a part of output schema of UDO.</span></span>

### <a name="functional-limitations"></a><span data-ttu-id="9bfbf-129">Funktionella begränsningar</span><span class="sxs-lookup"><span data-stu-id="9bfbf-129">Functional limitations</span></span>
* <span data-ttu-id="9bfbf-130">R-motorn kan inte initieras två gånger i samma process.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-130">The R Engine can't be instantiated twice in the same process.</span></span> 
* <span data-ttu-id="9bfbf-131">U-SQL stöder för närvarande inte Combiner UDO för förutsägelse partitionerade modeller som skapats med Reducer UDO.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-131">Currently, U-SQL does not support Combiner UDOs for prediction using partitioned models generated using Reducer UDOs.</span></span> <span data-ttu-id="9bfbf-132">Användare kan deklarera partitionerade modeller som resurs och använda dem i sina R-skript (se exempelkod `ExtR_PredictUsingLMRawStringReducer.usql`)</span><span class="sxs-lookup"><span data-stu-id="9bfbf-132">Users can declare the partitioned models as resource and use them in their R Script (see sample code `ExtR_PredictUsingLMRawStringReducer.usql`)</span></span>

### <a name="r-versions"></a><span data-ttu-id="9bfbf-133">R-versioner</span><span class="sxs-lookup"><span data-stu-id="9bfbf-133">R Versions</span></span>
<span data-ttu-id="9bfbf-134">Endast R 3.2.2 stöds.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-134">Only R 3.2.2 is supported.</span></span>

### <a name="standard-r-modules"></a><span data-ttu-id="9bfbf-135">Standard R-moduler</span><span class="sxs-lookup"><span data-stu-id="9bfbf-135">Standard R modules</span></span>

    base
    boot
    Class
    Cluster
    codetools
    compiler
    datasets
    doParallel
    doRSR
    foreach
    foreign
    Graphics
    grDevices
    grid
    iterators
    KernSmooth
    lattice
    MASS
    Matrix
    Methods
    mgcv
    nlme
    Nnet
    Parallel
    pkgXMLBuilder
    RevoIOQ
    revoIpe
    RevoMods
    RevoPemaR
    RevoRpeConnector
    RevoRsrConnector
    RevoScaleR
    RevoTreeView
    RevoUtils
    RevoUtilsMath
    Rpart
    RUnit
    spatial
    splines
    Stats
    stats4
    survival
    Tcltk
    Tools
    translations
    utils
    XML

### <a name="input-and-output-size-limitations"></a><span data-ttu-id="9bfbf-136">Indata och utdata storleksbegränsningar</span><span class="sxs-lookup"><span data-stu-id="9bfbf-136">Input and Output size limitations</span></span>
<span data-ttu-id="9bfbf-137">Varje nod har en begränsad mängd minne som tilldelats den.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-137">Every vertex has a limited amount of memory assigned to it.</span></span> <span data-ttu-id="9bfbf-138">Eftersom inkommande och utgående DataFrames måste finnas i minnet i R-koden, får inte den totala storleken för ingående och utgående överskrida 500 MB.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-138">Because the input and output DataFrames must exist in memory in the R code, the total size for the input and output cannot exceed 500 MB.</span></span>

### <a name="sample-code"></a><span data-ttu-id="9bfbf-139">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="9bfbf-139">Sample Code</span></span>
<span data-ttu-id="9bfbf-140">Flera exempelkod är tillgänglig i ditt Data Lake Store-konto när du har installerat U-SQL Advanced Analytics extensions.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-140">More sample code is available in your Data Lake Store account after you install the U-SQL Advanced Analytics extensions.</span></span> <span data-ttu-id="9bfbf-141">Sökvägen för flera exempelkod är: `<your_account_address>/usqlext/samples/R`.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-141">The path for more sample code is: `<your_account_address>/usqlext/samples/R`.</span></span> 

## <a name="deploying-custom-r-modules-with-u-sql"></a><span data-ttu-id="9bfbf-142">Distribuera anpassade R-moduler med U-SQL</span><span class="sxs-lookup"><span data-stu-id="9bfbf-142">Deploying Custom R modules with U-SQL</span></span>

<span data-ttu-id="9bfbf-143">Först skapar en anpassad R-modul och zip den och sedan ladda upp den komprimerade anpassa R-modulfilen ADL-arkivet.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-143">First, create an R custom module and zip it and then upload the zipped R custom module file to your ADL store.</span></span> <span data-ttu-id="9bfbf-144">I det här exemplet kommer vi att överföra magittr_1.5.zip till roten i ADLS standardkontot för kontot ADLA vi använder.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-144">In the example, we will upload magittr_1.5.zip to the root of the default ADLS account for the ADLA account we are using.</span></span> <span data-ttu-id="9bfbf-145">När du överför modulen till ADL store deklarera den som använder DISTRIBUERA RESURSEN för att göra den tillgänglig i U-SQL-skriptet och anropet `install.packages` att installera den.</span><span class="sxs-lookup"><span data-stu-id="9bfbf-145">Once you upload the module to ADL store, declare it as use DEPLOY RESOURCE to make it available in your U-SQL script and call `install.packages` to install it.</span></span>

    REFERENCE ASSEMBLY [ExtR];
    DEPLOY RESOURCE @"/magrittr_1.5.zip";

    DECLARE @IrisData string =  @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFileModelSummary string = @"/R/Output/CustomePackages.txt";

    // R script to run
    DECLARE @myRScript = @"
    # install the magrittr package,
    install.packages('magrittr_1.5.zip', repos = NULL),
    # load the magrittr package,
    require(magrittr),
    # demonstrate use of the magrittr package,
    2 %>% sqrt
    ";

    @InputData =
    EXTRACT SepalLength double,
    SepalWidth double,
    PetalLength double,
    PetalWidth double,
    Species string
    FROM @IrisData
    USING Extractors.Csv();

    @ExtendedData =
    SELECT 0 AS Par,
    *
    FROM @InputData;

    @RScriptOutput = REDUCE @ExtendedData ON Par
    PRODUCE Par, RowId int, ROutput string
    READONLY Par
    USING new Extension.R.Reducer(command:@myRScript, rReturnType:"charactermatrix");

    OUTPUT @RScriptOutput TO @OutputFileModelSummary USING Outputters.Tsv();

## <a name="next-steps"></a><span data-ttu-id="9bfbf-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9bfbf-146">Next Steps</span></span>
* [<span data-ttu-id="9bfbf-147">Översikt över Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="9bfbf-147">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="9bfbf-148">Utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9bfbf-148">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="9bfbf-149">Med hjälp av U-SQL-fönstrets funktioner för Azure Data Lake Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="9bfbf-149">Using U-SQL window functions for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-use-window-functions.md)
