---
title: "aaaTest och felsökningsloggar U-SQL-jobb med hjälp av lokala kör och hello Azure Data Lake U-SQL-SDK | Microsoft Docs"
description: "Lär dig hur toouse Azure Data Lake-verktyg för Visual Studio och hello Azure Data Lake U-SQL-SDK tootest och debug U-SQL-jobb på din lokala arbetsstationen."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/15/2016
ms.author: yanacai
ms.openlocfilehash: be04558a504acf6a088e207608ee2d4a011d3ffc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="test-and-debug-u-sql-jobs-by-using-local-run-and-hello-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="c1be4-103">Testa och felsöka U-SQL-jobb med hjälp av lokal kör och hello Azure Data Lake U-SQL-SDK</span><span class="sxs-lookup"><span data-stu-id="c1be4-103">Test and debug U-SQL jobs by using local run and hello Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="c1be4-104">Du kan använda Azure Data Lake-verktyg för Visual Studio och hello Azure Data Lake U-SQL-SDK toorun U-SQL-jobb på din arbetsstation, precis som i hello Azure Data Lake-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c1be4-104">You can use Azure Data Lake Tools for Visual Studio and hello Azure Data Lake U-SQL SDK toorun U-SQL jobs on your workstation, just as you can in hello Azure Data Lake service.</span></span> <span data-ttu-id="c1be4-105">Med de här två funktionerna som körs lokalt kan du spara tid när du testar och felsöker dina U-SQL-jobb.</span><span class="sxs-lookup"><span data-stu-id="c1be4-105">These two local-run features save you time in testing and debugging your U-SQL jobs.</span></span>

## <a name="understand-hello-data-root-folder-and-hello-file-path"></a><span data-ttu-id="c1be4-106">Förstå hello data-rotmappen och hello filsökväg</span><span class="sxs-lookup"><span data-stu-id="c1be4-106">Understand hello data-root folder and hello file path</span></span>

<span data-ttu-id="c1be4-107">Både lokal körning och hello U-SQL-SDK kräver en data-rotmappen.</span><span class="sxs-lookup"><span data-stu-id="c1be4-107">Both local run and hello U-SQL SDK require a data-root folder.</span></span> <span data-ttu-id="c1be4-108">hello data-rotmappen är ett ”lokalt Arkiv” för hello lokala beräkningskonto.</span><span class="sxs-lookup"><span data-stu-id="c1be4-108">hello data-root folder is a "local store" for hello local compute account.</span></span> <span data-ttu-id="c1be4-109">Det är likvärdiga toohello Azure Data Lake Store-konto för Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="c1be4-109">It's equivalent toohello Azure Data Lake Store account of a Data Lake Analytics account.</span></span> <span data-ttu-id="c1be4-110">Växla tooa olika data-rotmappen som är precis som växlar tooa olika store-konto.</span><span class="sxs-lookup"><span data-stu-id="c1be4-110">Switching tooa different data-root folder is just like switching tooa different store account.</span></span> <span data-ttu-id="c1be4-111">Om du vill tooaccess ofta delas data med olika dataroten mappar, måste du använda absoluta sökvägar i skript.</span><span class="sxs-lookup"><span data-stu-id="c1be4-111">If you want tooaccess commonly shared data with different data-root folders, you must use absolute paths in your scripts.</span></span> <span data-ttu-id="c1be4-112">Skapa filen system symboliska länkar (till exempel **mklink** på NTFS) under hello-dataroten mappen toopoint toohello delade data.</span><span class="sxs-lookup"><span data-stu-id="c1be4-112">Or, create file system symbolic links (for example, **mklink** on NTFS) under hello data-root folder toopoint toohello shared data.</span></span>

<span data-ttu-id="c1be4-113">hello data-rotmappen används för att:</span><span class="sxs-lookup"><span data-stu-id="c1be4-113">hello data-root folder is used to:</span></span>

- <span data-ttu-id="c1be4-114">Lagra metadata, inklusive databaser, tabeller, tabellvärdesfunktioner (Tabellvärdesfunktioner) och sammansättningar.</span><span class="sxs-lookup"><span data-stu-id="c1be4-114">Store metadata, including databases, tables, table-valued functions (TVFs), and assemblies.</span></span>
- <span data-ttu-id="c1be4-115">Leta upp hello indata och utdata sökvägar som har definierats som relativa sökvägar i U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c1be4-115">Look up hello input and output paths that are defined as relative paths in U-SQL.</span></span> <span data-ttu-id="c1be4-116">Använda relativa sökvägar gör det enklare toodeploy tooAzure dina U-SQL-projekt.</span><span class="sxs-lookup"><span data-stu-id="c1be4-116">Using relative paths makes it easier toodeploy your U-SQL projects tooAzure.</span></span>

<span data-ttu-id="c1be4-117">Du kan använda både en relativ sökväg och en lokal absolut sökväg i U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="c1be4-117">You can use both a relative path and a local absolute path in U-SQL scripts.</span></span> <span data-ttu-id="c1be4-118">hello relativ sökväg är relativ toohello angivna data-sökvägen till rotmappen.</span><span class="sxs-lookup"><span data-stu-id="c1be4-118">hello relative path is relative toohello specified data-root folder path.</span></span> <span data-ttu-id="c1be4-119">Vi rekommenderar att du använder ”/” som hello sökväg avgränsare toomake skripten som är kompatibel med hello på serversidan.</span><span class="sxs-lookup"><span data-stu-id="c1be4-119">We recommend that you use "/" as hello path separator toomake your scripts compatible with hello server side.</span></span> <span data-ttu-id="c1be4-120">Här följer några exempel på relativa sökvägar och deras motsvarande absoluta sökvägar.</span><span class="sxs-lookup"><span data-stu-id="c1be4-120">Here are some examples of relative paths and their equivalent absolute paths.</span></span> <span data-ttu-id="c1be4-121">I det här exemplet är C:\LocalRunDataRoot hello data-rotmappen.</span><span class="sxs-lookup"><span data-stu-id="c1be4-121">In these examples, C:\LocalRunDataRoot is hello data-root folder.</span></span>

|<span data-ttu-id="c1be4-122">Relativ sökväg</span><span class="sxs-lookup"><span data-stu-id="c1be4-122">Relative path</span></span>|<span data-ttu-id="c1be4-123">Absolut sökväg</span><span class="sxs-lookup"><span data-stu-id="c1be4-123">Absolute path</span></span>|
|-------------|-------------|
|<span data-ttu-id="c1be4-124">/ABC/def/Input.csv</span><span class="sxs-lookup"><span data-stu-id="c1be4-124">/abc/def/input.csv</span></span> |<span data-ttu-id="c1be4-125">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="c1be4-125">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="c1be4-126">ABC/def/Input.csv</span><span class="sxs-lookup"><span data-stu-id="c1be4-126">abc/def/input.csv</span></span>  |<span data-ttu-id="c1be4-127">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="c1be4-127">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="c1be4-128">D:/ABC/def/Input.csv</span><span class="sxs-lookup"><span data-stu-id="c1be4-128">D:/abc/def/input.csv</span></span> |<span data-ttu-id="c1be4-129">D:\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="c1be4-129">D:\abc\def\input.csv</span></span>|

## <a name="use-local-run-from-visual-studio"></a><span data-ttu-id="c1be4-130">Använd lokal kör från Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c1be4-130">Use local run from Visual Studio</span></span>

<span data-ttu-id="c1be4-131">Data Lake-verktyg för Visual Studio innehåller ett U-SQL lokalt körningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c1be4-131">Data Lake Tools for Visual Studio provides a U-SQL local-run experience in Visual Studio.</span></span> <span data-ttu-id="c1be4-132">Med hjälp av den här funktionen kan du:</span><span class="sxs-lookup"><span data-stu-id="c1be4-132">By using this feature, you can:</span></span>

- <span data-ttu-id="c1be4-133">Kör en U-SQL-skript lokalt, tillsammans med C#-sammansättningar.</span><span class="sxs-lookup"><span data-stu-id="c1be4-133">Run a U-SQL script locally, along with C# assemblies.</span></span>
- <span data-ttu-id="c1be4-134">Felsöka en C#-sammansättning lokalt.</span><span class="sxs-lookup"><span data-stu-id="c1be4-134">Debug a C# assembly locally.</span></span>
- <span data-ttu-id="c1be4-135">Skapa, visa och ta bort kataloger för U-SQL (lokala databaser, sammansättningar, scheman och tabeller) från Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="c1be4-135">Create, view, and delete U-SQL catalogs (local databases, assemblies, schemas, and tables) from Server Explorer.</span></span> <span data-ttu-id="c1be4-136">Du kan också hitta hello lokala katalogen också från Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="c1be4-136">You can also find hello local catalog also from Server Explorer.</span></span>

    ![Data Lake-verktyg för Visual Studio lokala kör lokal katalog](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-local-catalog.png)

<span data-ttu-id="c1be4-138">hello Data Lake-verktyg installationsprogrammet skapar en C:\LocalRunRoot mappen toobe som används som hello standard data-rotmappen.</span><span class="sxs-lookup"><span data-stu-id="c1be4-138">hello Data Lake Tools installer creates a C:\LocalRunRoot folder toobe used as hello default data-root folder.</span></span> <span data-ttu-id="c1be4-139">hello standard lokala kör parallellitet är 1.</span><span class="sxs-lookup"><span data-stu-id="c1be4-139">hello default local-run parallelism is 1.</span></span>

### <a name="tooconfigure-local-run-in-visual-studio"></a><span data-ttu-id="c1be4-140">tooconfigure lokala kör i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c1be4-140">tooconfigure local run in Visual Studio</span></span>

1. <span data-ttu-id="c1be4-141">Öppna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c1be4-141">Open Visual Studio.</span></span>
2. <span data-ttu-id="c1be4-142">Öppna **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c1be4-142">Open **Server Explorer**.</span></span>
3. <span data-ttu-id="c1be4-143">Expandera **Azure** > **Datasjöanalys**.</span><span class="sxs-lookup"><span data-stu-id="c1be4-143">Expand **Azure** > **Data Lake Analytics**.</span></span>
4. <span data-ttu-id="c1be4-144">Klicka på hello **Datasjö** -menyn och klicka sedan på **alternativ och inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="c1be4-144">Click hello **Data Lake** menu, and then click **Options and Settings**.</span></span>
5. <span data-ttu-id="c1be4-145">Expandera i hello vänster träd **Azure Data Lake**, och expandera sedan **allmänna**.</span><span class="sxs-lookup"><span data-stu-id="c1be4-145">In hello left tree, expand **Azure Data Lake**, and then expand **General**.</span></span>

    ![Konfigurera inställningar för data Lake-verktyg för Visual Studio kör lokalt](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-configure.png)

<span data-ttu-id="c1be4-147">Ett Visual Studio U-SQL-projekt krävs för att utföra lokala kör.</span><span class="sxs-lookup"><span data-stu-id="c1be4-147">A Visual Studio U-SQL project is required for performing local run.</span></span> <span data-ttu-id="c1be4-148">Den här delen skiljer sig från att köra U-SQL-skript från Azure.</span><span class="sxs-lookup"><span data-stu-id="c1be4-148">This part is different from running U-SQL scripts from Azure.</span></span>

### <a name="toorun-a-u-sql-script-locally"></a><span data-ttu-id="c1be4-149">toorun U-SQL-skript lokalt</span><span class="sxs-lookup"><span data-stu-id="c1be4-149">toorun a U-SQL script locally</span></span>
1. <span data-ttu-id="c1be4-150">Öppna projektet U-SQL Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c1be4-150">From Visual Studio, open your U-SQL project.</span></span>   
2. <span data-ttu-id="c1be4-151">Högerklicka på ett U-SQL-skript i Solution Explorer och klicka sedan på **skicka skript**.</span><span class="sxs-lookup"><span data-stu-id="c1be4-151">Right-click a U-SQL script in Solution Explorer, and then click **Submit Script**.</span></span>
3. <span data-ttu-id="c1be4-152">Välj **(lokal)** som hello Analytics-kontot toorun skriptet lokalt.</span><span class="sxs-lookup"><span data-stu-id="c1be4-152">Select **(Local)** as hello Analytics account toorun your script locally.</span></span>
<span data-ttu-id="c1be4-153">Du kan också klicka på hello **(lokal)** konto på hello upp i skriptfönstret och klicka sedan på **skicka** (eller använda hello Ctrl + F5 kortkommandot).</span><span class="sxs-lookup"><span data-stu-id="c1be4-153">You can also click hello **(Local)** account on hello top of script window, and then click **Submit** (or use hello Ctrl + F5 keyboard shortcut).</span></span>

    ![Data Lake-verktyg för Visual Studio lokala kör skicka jobb](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-submit-job.png)

### <a name="debug-scripts-and-c-assemblies-locally"></a><span data-ttu-id="c1be4-155">Felsök skript och C#-sammansättningar lokalt</span><span class="sxs-lookup"><span data-stu-id="c1be4-155">Debug scripts and C# assemblies locally</span></span>

<span data-ttu-id="c1be4-156">Du kan felsöka C#-sammansättningar utan att skicka och registrera den tooAzure Data Lake Analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c1be4-156">You can debug C# assemblies without submitting and registering it tooAzure Data Lake Analytics Service.</span></span> <span data-ttu-id="c1be4-157">Du kan ange brytpunkter i både hello koden bakom filen och ett refererat C#-projekt.</span><span class="sxs-lookup"><span data-stu-id="c1be4-157">You can set breakpoints in both hello code behind file and in a referenced C# project.</span></span>

#### <a name="toodebug-local-code-in-code-behind-file"></a><span data-ttu-id="c1be4-158">toodebug lokal kod i koden bakom filen</span><span class="sxs-lookup"><span data-stu-id="c1be4-158">toodebug local code in code behind file</span></span>

1. <span data-ttu-id="c1be4-159">Ange brytpunkter i hello koden bakom filen.</span><span class="sxs-lookup"><span data-stu-id="c1be4-159">Set breakpoints in hello code behind file.</span></span>
2. <span data-ttu-id="c1be4-160">Tryck på F5 toodebug hello skript lokalt.</span><span class="sxs-lookup"><span data-stu-id="c1be4-160">Press F5 toodebug hello script locally.</span></span>

> [!NOTE]
   > <span data-ttu-id="c1be4-161">hello följande procedur fungerar bara i Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="c1be4-161">hello following procedure only works in Visual Studio 2015.</span></span> <span data-ttu-id="c1be4-162">I äldre Visual Studio måste toomanually lägga till hello pdb-filer.</span><span class="sxs-lookup"><span data-stu-id="c1be4-162">In older Visual Studio you may need toomanually add hello pdb files.</span></span>  
   >
   >

#### <a name="toodebug-local-code-in-a-referenced-c-project"></a><span data-ttu-id="c1be4-163">toodebug lokal kod i ett refererat C#-projekt</span><span class="sxs-lookup"><span data-stu-id="c1be4-163">toodebug local code in a referenced C# project</span></span>

1. <span data-ttu-id="c1be4-164">Skapa ett C#-sammansättningsprojekt och skapa det toogenerate hello utdata DLL-filen.</span><span class="sxs-lookup"><span data-stu-id="c1be4-164">Create a C# Assembly project, and build it toogenerate hello output dll.</span></span>
2. <span data-ttu-id="c1be4-165">Registrera hello DLL-filen med ett U-SQL-uttryck:</span><span class="sxs-lookup"><span data-stu-id="c1be4-165">Register hello dll using a U-SQL statement:</span></span>

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. <span data-ttu-id="c1be4-166">Ange brytpunkter i hello C#-kod.</span><span class="sxs-lookup"><span data-stu-id="c1be4-166">Set breakpoints in hello C# code.</span></span>
4. <span data-ttu-id="c1be4-167">Tryck på F5 toodebug hello skriptet med referens hello C# DLL-filen lokalt.</span><span class="sxs-lookup"><span data-stu-id="c1be4-167">Press F5 toodebug hello script with referencing hello C# dll locally.</span></span>

## <a name="use-local-run-from-hello-data-lake-u-sql-sdk"></a><span data-ttu-id="c1be4-168">Använd lokal kör från hello Data Lake U-SQL-SDK</span><span class="sxs-lookup"><span data-stu-id="c1be4-168">Use local run from hello Data Lake U-SQL SDK</span></span>

<span data-ttu-id="c1be4-169">Dessutom toorunning U-SQL-skript lokalt med hjälp av Visual Studio kan du använda hello Azure Data Lake U-SQL-SDK toorun U-SQL-skript lokalt med kommandoraden och API-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="c1be4-169">In addition toorunning U-SQL scripts locally by using Visual Studio, you can use hello Azure Data Lake U-SQL SDK toorun U-SQL scripts locally with command-line and programming interfaces.</span></span> <span data-ttu-id="c1be4-170">Du kan skala din lokal U-SQL-test via dessa.</span><span class="sxs-lookup"><span data-stu-id="c1be4-170">Through these, you can scale your U-SQL local test.</span></span>

<span data-ttu-id="c1be4-171">Lär dig mer om [Azure Data Lake U-SQL-SDK](data-lake-analytics-u-sql-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="c1be4-171">Learn more about [Azure Data Lake U-SQL SDK](data-lake-analytics-u-sql-sdk.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="c1be4-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c1be4-172">Next steps</span></span>

* <span data-ttu-id="c1be4-173">toosee en mer komplex fråga, se [analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="c1be4-173">toosee a more complex query, see [Analyze website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="c1be4-174">tooview jobbinformation finns [Använd jobbet webbläsare och visa jobb för Azure Data Lake Analytics-jobb](data-lake-analytics-data-lake-tools-view-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="c1be4-174">tooview job details, see [Use Job Browser and Job View for Azure Data Lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md).</span></span>
* <span data-ttu-id="c1be4-175">toouse hello vertex vy, se [Använd hello Vertex vy i Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span><span class="sxs-lookup"><span data-stu-id="c1be4-175">toouse hello vertex execution view, see [Use hello Vertex Execution View in Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span></span>
