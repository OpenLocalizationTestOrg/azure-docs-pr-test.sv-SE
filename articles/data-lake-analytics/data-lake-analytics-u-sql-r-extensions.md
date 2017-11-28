---
title: aaaExtend U-SQL-skript med R i Azure Data Lake Analytics | Microsoft Docs
description: "Lär dig hur toorun R code i U-SQL-skript"
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
ms.openlocfilehash: 24affd4963a08d30a7111b49af388e9c1268430e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-r"></a><span data-ttu-id="131ee-103">Självstudier: Kom igång med att utöka U-SQL med R</span><span class="sxs-lookup"><span data-stu-id="131ee-103">Tutorial: Get started with extending U-SQL with R</span></span>

<span data-ttu-id="131ee-104">hello som följande exempel visar hello grundläggande steg för att distribuera R-koden:</span><span class="sxs-lookup"><span data-stu-id="131ee-104">hello following example illustrates hello basic steps for deploying R code:</span></span>
* <span data-ttu-id="131ee-105">Använd hello `REFERENCE ASSEMBLY` instruktionen tooenable R-tillägg för hello U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="131ee-105">Use hello `REFERENCE ASSEMBLY` statement tooenable R extensions for hello U-SQL Script.</span></span>
* <span data-ttu-id="131ee-106">Använd den` REDUCE` åtgärden toopartition hello mata in data på en nyckel.</span><span class="sxs-lookup"><span data-stu-id="131ee-106">Use the` REDUCE` operation toopartition hello input data on a key.</span></span>
* <span data-ttu-id="131ee-107">hello R-tillägg för U-SQL är en inbyggd reducer (`Extension.R.Reducer`) R-koden körs på varje nod som tilldelats toohello reducer.</span><span class="sxs-lookup"><span data-stu-id="131ee-107">hello R extensions for U-SQL include a built-in reducer (`Extension.R.Reducer`) that runs R code on each vertex assigned toohello reducer.</span></span> 
* <span data-ttu-id="131ee-108">Användning av dedikerade med namnet ramar kallas `inputFromUSQL` och `outputToUSQL `respektive toopass data mellan U-SQL och R. indata och utdata DataFrame identifierarnamn korrigeras (det vill säga användare kan inte ändra dessa fördefinierade namn på indata och utdata DataFrame identifierare).</span><span class="sxs-lookup"><span data-stu-id="131ee-108">Usage of dedicated named data frames called `inputFromUSQL` and `outputToUSQL `respectively toopass data between U-SQL and R. Input and output DataFrame identifier names are fixed (that is, users cannot change these predefined names of input and output DataFrame identifiers).</span></span>

## <a name="embedding-r-code-in-hello-u-sql-script"></a><span data-ttu-id="131ee-109">Bädda in R-koden i hello U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="131ee-109">Embedding R code in hello U-SQL script</span></span>

<span data-ttu-id="131ee-110">Du kan infogad hello R kod U-SQL-skript med hello Kommandoparametern av hello `Extension.R.Reducer`.</span><span class="sxs-lookup"><span data-stu-id="131ee-110">You can inline hello R code your U-SQL script by using hello command parameter of hello `Extension.R.Reducer`.</span></span> <span data-ttu-id="131ee-111">Du kan exempelvis deklarera hello R-skriptet som en string-variabel och skicka den som en parameter toohello Reducer.</span><span class="sxs-lookup"><span data-stu-id="131ee-111">For example, you can declare hello R script as a string variable and pass it as a parameter toohello Reducer.</span></span>


    REFERENCE ASSEMBLY [ExtR];
    
    DECLARE @myRScript = @"
    inputFromUSQL$Species = as.factor(inputFromUSQL$Species)
    lm.fit=lm(unclass(Species)~.-Par, data=inputFromUSQL)
    #do not return readonly columns and make sure that hello column names are hello same in usql and r scripts,
    outputToUSQL=data.frame(summary(lm.fit)$coefficients)
    colnames(outputToUSQL) <- c(""Estimate"", ""StdError"", ""tValue"", ""Pr"")
    outputToUSQL
    ";
    
    @RScriptOutput = REDUCE … USING new Extension.R.Reducer(command:@myRScript, rReturnType:"dataframe");

## <a name="keep-hello-r-code-in-a-separate-file-and-reference-it--hello-u-sql-script"></a><span data-ttu-id="131ee-112">Behåll hello R-koden i en separat fil och hänvisar det hello U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="131ee-112">Keep hello R code in a separate file and reference it  hello U-SQL script</span></span>

<span data-ttu-id="131ee-113">hello följande exempel illustrerar användning av en mer komplex.</span><span class="sxs-lookup"><span data-stu-id="131ee-113">hello following example illustrates a more complex usage.</span></span> <span data-ttu-id="131ee-114">I det här fallet distribueras hello R kod som en resurs som hello U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="131ee-114">In this case, hello R code is deployed as a RESOURCE that is hello U-SQL script.</span></span>

<span data-ttu-id="131ee-115">Spara den här R-koden som en separat fil.</span><span class="sxs-lookup"><span data-stu-id="131ee-115">Save this R code as a separate file.</span></span>

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

<span data-ttu-id="131ee-116">Använda toodeploy ett U-SQL-skript som R-skriptet med hello DISTRIBUERA resurs-instruktion.</span><span class="sxs-lookup"><span data-stu-id="131ee-116">Use a U-SQL script toodeploy that R script with hello DEPLOY RESOURCE statement.</span></span>

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
        OUTPUT @RScriptOutput too@OutputFilePredictions USING Outputters.Tsv();

## <a name="how-r-integrates-with-u-sql"></a><span data-ttu-id="131ee-117">Hur R kan integreras med U-SQL</span><span class="sxs-lookup"><span data-stu-id="131ee-117">How R Integrates with U-SQL</span></span>

### <a name="datatypes"></a><span data-ttu-id="131ee-118">Datatyper</span><span class="sxs-lookup"><span data-stu-id="131ee-118">Datatypes</span></span>
* <span data-ttu-id="131ee-119">Konverteras som sträng och numeriska kolumner från U-SQL-mellan R DataFrame och U-SQL [typer som stöds: `double`, `string`, `bool`, `integer`, `byte`].</span><span class="sxs-lookup"><span data-stu-id="131ee-119">String and numeric columns from U-SQL are converted as-is between R DataFrame and U-SQL [supported types: `double`, `string`, `bool`, `integer`, `byte`].</span></span>
* <span data-ttu-id="131ee-120">Hej `Factor` stöds inte i U-SQL.</span><span class="sxs-lookup"><span data-stu-id="131ee-120">hello `Factor` datatype is not supported in U-SQL.</span></span>
* <span data-ttu-id="131ee-121">`byte[]`måste serialiseras som en base64-kodad `string`.</span><span class="sxs-lookup"><span data-stu-id="131ee-121">`byte[]` must be serialized as a base64-encoded `string`.</span></span>
* <span data-ttu-id="131ee-122">U-SQL-strängar kan vara konverterade toofactors i R-koden när U-SQL skapar R inkommande dataframe eller genom att ange hello reducer parametern `stringsAsFactors: true`.</span><span class="sxs-lookup"><span data-stu-id="131ee-122">U-SQL strings can be converted toofactors in R code, once U-SQL create R input dataframe or by setting hello reducer parameter `stringsAsFactors: true`.</span></span>

### <a name="schemas"></a><span data-ttu-id="131ee-123">Scheman</span><span class="sxs-lookup"><span data-stu-id="131ee-123">Schemas</span></span>
* <span data-ttu-id="131ee-124">U-SQL-datauppsättningar kan inte ha dubbletter av kolumnnamn.</span><span class="sxs-lookup"><span data-stu-id="131ee-124">U-SQL datasets cannot have duplicate column names.</span></span>
* <span data-ttu-id="131ee-125">U-SQL datauppsättningar kolumnnamn måste vara strängar.</span><span class="sxs-lookup"><span data-stu-id="131ee-125">U-SQL datasets column names must be strings.</span></span>
* <span data-ttu-id="131ee-126">Kolumnnamnen måste hello samma i U-SQL och R-skript.</span><span class="sxs-lookup"><span data-stu-id="131ee-126">Column names must be hello same in U-SQL and R scripts.</span></span>
* <span data-ttu-id="131ee-127">ReadOnly-kolumn kan inte ingå i hello utdata dataframe.</span><span class="sxs-lookup"><span data-stu-id="131ee-127">Readonly column cannot be part of hello output dataframe.</span></span> <span data-ttu-id="131ee-128">Eftersom readonly kolumner automatiskt matas in tillbaka i hello U-SQL-tabell om det är en del av utdataschemat för UDO.</span><span class="sxs-lookup"><span data-stu-id="131ee-128">Because readonly columns are automatically injected back in hello U-SQL table if it is a part of output schema of UDO.</span></span>

### <a name="functional-limitations"></a><span data-ttu-id="131ee-129">Funktionella begränsningar</span><span class="sxs-lookup"><span data-stu-id="131ee-129">Functional limitations</span></span>
* <span data-ttu-id="131ee-130">hello R-motorn kan inte initieras två gånger i hello samma process.</span><span class="sxs-lookup"><span data-stu-id="131ee-130">hello R Engine can't be instantiated twice in hello same process.</span></span> 
* <span data-ttu-id="131ee-131">U-SQL stöder för närvarande inte Combiner UDO för förutsägelse partitionerade modeller som skapats med Reducer UDO.</span><span class="sxs-lookup"><span data-stu-id="131ee-131">Currently, U-SQL does not support Combiner UDOs for prediction using partitioned models generated using Reducer UDOs.</span></span> <span data-ttu-id="131ee-132">Användare kan deklarera hello partitionerad modeller som resurs och använda dem i sina R-skript (se exempelkod `ExtR_PredictUsingLMRawStringReducer.usql`)</span><span class="sxs-lookup"><span data-stu-id="131ee-132">Users can declare hello partitioned models as resource and use them in their R Script (see sample code `ExtR_PredictUsingLMRawStringReducer.usql`)</span></span>

### <a name="r-versions"></a><span data-ttu-id="131ee-133">R-versioner</span><span class="sxs-lookup"><span data-stu-id="131ee-133">R Versions</span></span>
<span data-ttu-id="131ee-134">Endast R 3.2.2 stöds.</span><span class="sxs-lookup"><span data-stu-id="131ee-134">Only R 3.2.2 is supported.</span></span>

### <a name="standard-r-modules"></a><span data-ttu-id="131ee-135">Standard R-moduler</span><span class="sxs-lookup"><span data-stu-id="131ee-135">Standard R modules</span></span>

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

### <a name="input-and-output-size-limitations"></a><span data-ttu-id="131ee-136">Indata och utdata storleksbegränsningar</span><span class="sxs-lookup"><span data-stu-id="131ee-136">Input and Output size limitations</span></span>
<span data-ttu-id="131ee-137">Varje nod har en begränsad mängd minne som tilldelats tooit.</span><span class="sxs-lookup"><span data-stu-id="131ee-137">Every vertex has a limited amount of memory assigned tooit.</span></span> <span data-ttu-id="131ee-138">Eftersom hello inkommande och utgående DataFrames måste finnas i minnet i hello R-koden, får inte hello total storlek för hello indata och utdata överskrida 500 MB.</span><span class="sxs-lookup"><span data-stu-id="131ee-138">Because hello input and output DataFrames must exist in memory in hello R code, hello total size for hello input and output cannot exceed 500 MB.</span></span>

### <a name="sample-code"></a><span data-ttu-id="131ee-139">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="131ee-139">Sample Code</span></span>
<span data-ttu-id="131ee-140">Flera exempelkod är tillgänglig i ditt Data Lake Store-konto när du har installerat hello U-SQL Advanced Analytics extensions.</span><span class="sxs-lookup"><span data-stu-id="131ee-140">More sample code is available in your Data Lake Store account after you install hello U-SQL Advanced Analytics extensions.</span></span> <span data-ttu-id="131ee-141">hello-sökvägen för flera exempelkod är: `<your_account_address>/usqlext/samples/R`.</span><span class="sxs-lookup"><span data-stu-id="131ee-141">hello path for more sample code is: `<your_account_address>/usqlext/samples/R`.</span></span> 

## <a name="deploying-custom-r-modules-with-u-sql"></a><span data-ttu-id="131ee-142">Distribuera anpassade R-moduler med U-SQL</span><span class="sxs-lookup"><span data-stu-id="131ee-142">Deploying Custom R modules with U-SQL</span></span>

<span data-ttu-id="131ee-143">Först skapar en anpassad R-modul och zip den och överför hello zippade R anpassad modul tooyour ADL-lagring.</span><span class="sxs-lookup"><span data-stu-id="131ee-143">First, create an R custom module and zip it and then upload hello zipped R custom module file tooyour ADL store.</span></span> <span data-ttu-id="131ee-144">I exemplet hello kommer vi att överföra magittr_1.5.zip toohello rot hello ADLS standardkontot för hello ADLA konto vi använder.</span><span class="sxs-lookup"><span data-stu-id="131ee-144">In hello example, we will upload magittr_1.5.zip toohello root of hello default ADLS account for hello ADLA account we are using.</span></span> <span data-ttu-id="131ee-145">När du överför hello modulen tooADL store deklarera den som använder DISTRIBUERA resurs toomake den tillgänglig i U-SQL-skriptet och anropet `install.packages` tooinstall den.</span><span class="sxs-lookup"><span data-stu-id="131ee-145">Once you upload hello module tooADL store, declare it as use DEPLOY RESOURCE toomake it available in your U-SQL script and call `install.packages` tooinstall it.</span></span>

    REFERENCE ASSEMBLY [ExtR];
    DEPLOY RESOURCE @"/magrittr_1.5.zip";

    DECLARE @IrisData string =  @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFileModelSummary string = @"/R/Output/CustomePackages.txt";

    // R script toorun
    DECLARE @myRScript = @"
    # install hello magrittr package,
    install.packages('magrittr_1.5.zip', repos = NULL),
    # load hello magrittr package,
    require(magrittr),
    # demonstrate use of hello magrittr package,
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

    OUTPUT @RScriptOutput too@OutputFileModelSummary USING Outputters.Tsv();

## <a name="next-steps"></a><span data-ttu-id="131ee-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="131ee-146">Next Steps</span></span>
* [<span data-ttu-id="131ee-147">Översikt över Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="131ee-147">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="131ee-148">Utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="131ee-148">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="131ee-149">Med hjälp av U-SQL-fönstrets funktioner för Azure Data Lake Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="131ee-149">Using U-SQL window functions for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-use-window-functions.md)
