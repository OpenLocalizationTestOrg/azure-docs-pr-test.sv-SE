---
title: "Testa och felsöka U-SQL-jobb med hjälp av lokal körning och Azure Data Lake U-SQL-SDK | Microsoft Docs"
description: "Lär dig hur du använder Azure Data Lake-verktyg för Visual Studio och Azure Data Lake U-SQL-SDK för att testa och felsöka U-SQL-jobb på din lokala arbetsstationen."
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
ms.openlocfilehash: 771a96df5cc66bac46e7144785be8cc072b57b31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="test-and-debug-u-sql-jobs-by-using-local-run-and-the-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="44685-103">Testa och felsöka U-SQL-jobb med hjälp av lokal kör och Azure Data Lake U-SQL-SDK</span><span class="sxs-lookup"><span data-stu-id="44685-103">Test and debug U-SQL jobs by using local run and the Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="44685-104">Du kan använda Azure Data Lake Tools för Visual Studio och Azure Data Lake U-SQL SDK för att köra U-SQL-jobb på din arbetsstation, som i Azure Data Lake-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="44685-104">You can use Azure Data Lake Tools for Visual Studio and the Azure Data Lake U-SQL SDK to run U-SQL jobs on your workstation, just as you can in the Azure Data Lake service.</span></span> <span data-ttu-id="44685-105">Med de här två funktionerna som körs lokalt kan du spara tid när du testar och felsöker dina U-SQL-jobb.</span><span class="sxs-lookup"><span data-stu-id="44685-105">These two local-run features save you time in testing and debugging your U-SQL jobs.</span></span>

## <a name="understand-the-data-root-folder-and-the-file-path"></a><span data-ttu-id="44685-106">Förstå data till rotmappen och sökvägen till filen</span><span class="sxs-lookup"><span data-stu-id="44685-106">Understand the data-root folder and the file path</span></span>

<span data-ttu-id="44685-107">Både lokal körning och U-SQL-SDK kräver en data-rotmappen.</span><span class="sxs-lookup"><span data-stu-id="44685-107">Both local run and the U-SQL SDK require a data-root folder.</span></span> <span data-ttu-id="44685-108">Data-rotmappen är ”lokalt Arkiv” för den lokala beräkningskonto.</span><span class="sxs-lookup"><span data-stu-id="44685-108">The data-root folder is a "local store" for the local compute account.</span></span> <span data-ttu-id="44685-109">Det motsvarar att Azure Data Lake Store-konto för Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="44685-109">It's equivalent to the Azure Data Lake Store account of a Data Lake Analytics account.</span></span> <span data-ttu-id="44685-110">Växla till en annan data-rotmapp är precis som om du växlar till en annan store-konto.</span><span class="sxs-lookup"><span data-stu-id="44685-110">Switching to a different data-root folder is just like switching to a different store account.</span></span> <span data-ttu-id="44685-111">Om du vill komma åt ofta delade data med olika dataroten mappar måste du använda absoluta sökvägar i skript.</span><span class="sxs-lookup"><span data-stu-id="44685-111">If you want to access commonly shared data with different data-root folders, you must use absolute paths in your scripts.</span></span> <span data-ttu-id="44685-112">Skapa filen system symboliska länkar (till exempel **mklink** på NTFS) under data till rotmappen att peka till delade data.</span><span class="sxs-lookup"><span data-stu-id="44685-112">Or, create file system symbolic links (for example, **mklink** on NTFS) under the data-root folder to point to the shared data.</span></span>

<span data-ttu-id="44685-113">Data-rotmappen används för att:</span><span class="sxs-lookup"><span data-stu-id="44685-113">The data-root folder is used to:</span></span>

- <span data-ttu-id="44685-114">Lagra metadata, inklusive databaser, tabeller, tabellvärdesfunktioner (Tabellvärdesfunktioner) och sammansättningar.</span><span class="sxs-lookup"><span data-stu-id="44685-114">Store metadata, including databases, tables, table-valued functions (TVFs), and assemblies.</span></span>
- <span data-ttu-id="44685-115">Leta upp de inkommande och utgående sökvägar som har definierats som relativa sökvägar i U-SQL.</span><span class="sxs-lookup"><span data-stu-id="44685-115">Look up the input and output paths that are defined as relative paths in U-SQL.</span></span> <span data-ttu-id="44685-116">Använda relativa sökvägar gör det enklare att distribuera dina U-SQL-projekt till Azure.</span><span class="sxs-lookup"><span data-stu-id="44685-116">Using relative paths makes it easier to deploy your U-SQL projects to Azure.</span></span>

<span data-ttu-id="44685-117">Du kan använda både en relativ sökväg och en lokal absolut sökväg i U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="44685-117">You can use both a relative path and a local absolute path in U-SQL scripts.</span></span> <span data-ttu-id="44685-118">Den relativa sökvägen är i förhållande till angivna data-sökvägen till rotmappen.</span><span class="sxs-lookup"><span data-stu-id="44685-118">The relative path is relative to the specified data-root folder path.</span></span> <span data-ttu-id="44685-119">Vi rekommenderar att du använder ”/” som sökväg avgränsare att göra skript som är kompatibel med servern.</span><span class="sxs-lookup"><span data-stu-id="44685-119">We recommend that you use "/" as the path separator to make your scripts compatible with the server side.</span></span> <span data-ttu-id="44685-120">Här följer några exempel på relativa sökvägar och deras motsvarande absoluta sökvägar.</span><span class="sxs-lookup"><span data-stu-id="44685-120">Here are some examples of relative paths and their equivalent absolute paths.</span></span> <span data-ttu-id="44685-121">I det här exemplet är C:\LocalRunDataRoot data till rotmappen.</span><span class="sxs-lookup"><span data-stu-id="44685-121">In these examples, C:\LocalRunDataRoot is the data-root folder.</span></span>

|<span data-ttu-id="44685-122">Relativ sökväg</span><span class="sxs-lookup"><span data-stu-id="44685-122">Relative path</span></span>|<span data-ttu-id="44685-123">Absolut sökväg</span><span class="sxs-lookup"><span data-stu-id="44685-123">Absolute path</span></span>|
|-------------|-------------|
|<span data-ttu-id="44685-124">/ABC/def/Input.csv</span><span class="sxs-lookup"><span data-stu-id="44685-124">/abc/def/input.csv</span></span> |<span data-ttu-id="44685-125">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="44685-125">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="44685-126">ABC/def/Input.csv</span><span class="sxs-lookup"><span data-stu-id="44685-126">abc/def/input.csv</span></span>  |<span data-ttu-id="44685-127">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="44685-127">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="44685-128">D:/ABC/def/Input.csv</span><span class="sxs-lookup"><span data-stu-id="44685-128">D:/abc/def/input.csv</span></span> |<span data-ttu-id="44685-129">D:\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="44685-129">D:\abc\def\input.csv</span></span>|

## <a name="use-local-run-from-visual-studio"></a><span data-ttu-id="44685-130">Använd lokal kör från Visual Studio</span><span class="sxs-lookup"><span data-stu-id="44685-130">Use local run from Visual Studio</span></span>

<span data-ttu-id="44685-131">Data Lake-verktyg för Visual Studio innehåller ett U-SQL lokalt körningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="44685-131">Data Lake Tools for Visual Studio provides a U-SQL local-run experience in Visual Studio.</span></span> <span data-ttu-id="44685-132">Med hjälp av den här funktionen kan du:</span><span class="sxs-lookup"><span data-stu-id="44685-132">By using this feature, you can:</span></span>

- <span data-ttu-id="44685-133">Kör en U-SQL-skript lokalt, tillsammans med C#-sammansättningar.</span><span class="sxs-lookup"><span data-stu-id="44685-133">Run a U-SQL script locally, along with C# assemblies.</span></span>
- <span data-ttu-id="44685-134">Felsöka en C#-sammansättning lokalt.</span><span class="sxs-lookup"><span data-stu-id="44685-134">Debug a C# assembly locally.</span></span>
- <span data-ttu-id="44685-135">Skapa, visa och ta bort kataloger för U-SQL (lokala databaser, sammansättningar, scheman och tabeller) från Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="44685-135">Create, view, and delete U-SQL catalogs (local databases, assemblies, schemas, and tables) from Server Explorer.</span></span> <span data-ttu-id="44685-136">Du kan också hitta den lokala katalogen också från Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="44685-136">You can also find the local catalog also from Server Explorer.</span></span>

    ![Data Lake-verktyg för Visual Studio lokala kör lokal katalog](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-local-catalog.png)

<span data-ttu-id="44685-138">Data Lake-verktyg installationsprogrammet skapar en C:\LocalRunRoot mapp som ska användas som standard data-rotmappen.</span><span class="sxs-lookup"><span data-stu-id="44685-138">The Data Lake Tools installer creates a C:\LocalRunRoot folder to be used as the default data-root folder.</span></span> <span data-ttu-id="44685-139">Standard lokala kör parallellitet är 1.</span><span class="sxs-lookup"><span data-stu-id="44685-139">The default local-run parallelism is 1.</span></span>

### <a name="to-configure-local-run-in-visual-studio"></a><span data-ttu-id="44685-140">Konfigurera lokal körning i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="44685-140">To configure local run in Visual Studio</span></span>

1. <span data-ttu-id="44685-141">Öppna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="44685-141">Open Visual Studio.</span></span>
2. <span data-ttu-id="44685-142">Öppna **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="44685-142">Open **Server Explorer**.</span></span>
3. <span data-ttu-id="44685-143">Expandera **Azure** > **Datasjöanalys**.</span><span class="sxs-lookup"><span data-stu-id="44685-143">Expand **Azure** > **Data Lake Analytics**.</span></span>
4. <span data-ttu-id="44685-144">Klicka på den **Data Lake** -menyn och klicka sedan på **alternativ och inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="44685-144">Click the **Data Lake** menu, and then click **Options and Settings**.</span></span>
5. <span data-ttu-id="44685-145">Expandera i vänster träd **Azure Data Lake**, och expandera sedan **allmänna**.</span><span class="sxs-lookup"><span data-stu-id="44685-145">In the left tree, expand **Azure Data Lake**, and then expand **General**.</span></span>

    ![Konfigurera inställningar för data Lake-verktyg för Visual Studio kör lokalt](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-configure.png)

<span data-ttu-id="44685-147">Ett Visual Studio U-SQL-projekt krävs för att utföra lokala kör.</span><span class="sxs-lookup"><span data-stu-id="44685-147">A Visual Studio U-SQL project is required for performing local run.</span></span> <span data-ttu-id="44685-148">Den här delen skiljer sig från att köra U-SQL-skript från Azure.</span><span class="sxs-lookup"><span data-stu-id="44685-148">This part is different from running U-SQL scripts from Azure.</span></span>

### <a name="to-run-a-u-sql-script-locally"></a><span data-ttu-id="44685-149">Att köra ett U-SQL-skript lokalt</span><span class="sxs-lookup"><span data-stu-id="44685-149">To run a U-SQL script locally</span></span>
1. <span data-ttu-id="44685-150">Öppna projektet U-SQL Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="44685-150">From Visual Studio, open your U-SQL project.</span></span>   
2. <span data-ttu-id="44685-151">Högerklicka på ett U-SQL-skript i Solution Explorer och klicka sedan på **skicka skript**.</span><span class="sxs-lookup"><span data-stu-id="44685-151">Right-click a U-SQL script in Solution Explorer, and then click **Submit Script**.</span></span>
3. <span data-ttu-id="44685-152">Välj **(lokal)** som Analytics-konto för att köra skriptet lokalt.</span><span class="sxs-lookup"><span data-stu-id="44685-152">Select **(Local)** as the Analytics account to run your script locally.</span></span>
<span data-ttu-id="44685-153">Du kan också klicka på **(lokal)** överst skriptfönstret-kontot och klicka sedan på **skicka** (eller Använd Ctrl + F5 kortkommandot).</span><span class="sxs-lookup"><span data-stu-id="44685-153">You can also click the **(Local)** account on the top of script window, and then click **Submit** (or use the Ctrl + F5 keyboard shortcut).</span></span>

    ![Data Lake-verktyg för Visual Studio lokala kör skicka jobb](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-submit-job.png)

### <a name="debug-scripts-and-c-assemblies-locally"></a><span data-ttu-id="44685-155">Felsök skript och C#-sammansättningar lokalt</span><span class="sxs-lookup"><span data-stu-id="44685-155">Debug scripts and C# assemblies locally</span></span>

<span data-ttu-id="44685-156">Du kan felsöka C#-sammansättningar utan att skicka och registrera dem till Azure Data Lake Analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="44685-156">You can debug C# assemblies without submitting and registering it to Azure Data Lake Analytics Service.</span></span> <span data-ttu-id="44685-157">Du kan ange brytpunkter i både koden bakom filen och ett refererat C#-projekt.</span><span class="sxs-lookup"><span data-stu-id="44685-157">You can set breakpoints in both the code behind file and in a referenced C# project.</span></span>

#### <a name="to-debug-local-code-in-code-behind-file"></a><span data-ttu-id="44685-158">Felsöka lokal kod i koden bakom filen</span><span class="sxs-lookup"><span data-stu-id="44685-158">To debug local code in code behind file</span></span>

1. <span data-ttu-id="44685-159">Ange brytpunkter i koden bakom filen.</span><span class="sxs-lookup"><span data-stu-id="44685-159">Set breakpoints in the code behind file.</span></span>
2. <span data-ttu-id="44685-160">Tryck på F5 för att felsöka skript lokalt.</span><span class="sxs-lookup"><span data-stu-id="44685-160">Press F5 to debug the script locally.</span></span>

> [!NOTE]
   > <span data-ttu-id="44685-161">Följande procedur fungerar bara i Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="44685-161">The following procedure only works in Visual Studio 2015.</span></span> <span data-ttu-id="44685-162">Du kan behöva lägga till PDB-filer manuellt i äldre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="44685-162">In older Visual Studio you may need to manually add the pdb files.</span></span>  
   >
   >

#### <a name="to-debug-local-code-in-a-referenced-c-project"></a><span data-ttu-id="44685-163">Om du vill felsöka lokal kod i ett refererat C#-projekt</span><span class="sxs-lookup"><span data-stu-id="44685-163">To debug local code in a referenced C# project</span></span>

1. <span data-ttu-id="44685-164">Skapa ett C#-sammansättningsprojekt och skapa det för att generera DLL-filen för utdata.</span><span class="sxs-lookup"><span data-stu-id="44685-164">Create a C# Assembly project, and build it to generate the output dll.</span></span>
2. <span data-ttu-id="44685-165">Registrera DLL-filen med hjälp av ett U-SQL-uttryck:</span><span class="sxs-lookup"><span data-stu-id="44685-165">Register the dll using a U-SQL statement:</span></span>

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. <span data-ttu-id="44685-166">Ange brytpunkter i C#-koden.</span><span class="sxs-lookup"><span data-stu-id="44685-166">Set breakpoints in the C# code.</span></span>
4. <span data-ttu-id="44685-167">Tryck på F5 för att felsöka skriptet med referens C#-DLL: en lokalt.</span><span class="sxs-lookup"><span data-stu-id="44685-167">Press F5 to debug the script with referencing the C# dll locally.</span></span>

## <a name="use-local-run-from-the-data-lake-u-sql-sdk"></a><span data-ttu-id="44685-168">Använd lokal körning från Data Lake U-SQL-SDK</span><span class="sxs-lookup"><span data-stu-id="44685-168">Use local run from the Data Lake U-SQL SDK</span></span>

<span data-ttu-id="44685-169">Du kan använda Azure Data Lake U-SQL-SDK för att köra U-SQL-skript lokalt med kommandoraden och programming interfaces förutom att köra U-SQL-skript lokalt med hjälp av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="44685-169">In addition to running U-SQL scripts locally by using Visual Studio, you can use the Azure Data Lake U-SQL SDK to run U-SQL scripts locally with command-line and programming interfaces.</span></span> <span data-ttu-id="44685-170">Du kan skala din lokal U-SQL-test via dessa.</span><span class="sxs-lookup"><span data-stu-id="44685-170">Through these, you can scale your U-SQL local test.</span></span>

<span data-ttu-id="44685-171">Lär dig mer om [Azure Data Lake U-SQL-SDK](data-lake-analytics-u-sql-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="44685-171">Learn more about [Azure Data Lake U-SQL SDK](data-lake-analytics-u-sql-sdk.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="44685-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="44685-172">Next steps</span></span>

* <span data-ttu-id="44685-173">Om du vill se en mer komplex fråga, se [analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="44685-173">To see a more complex query, see [Analyze website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="44685-174">Om du vill visa jobbinformation [Använd jobbet webbläsare och visa jobb för Azure Data Lake Analytics-jobb](data-lake-analytics-data-lake-tools-view-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="44685-174">To view job details, see [Use Job Browser and Job View for Azure Data Lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md).</span></span>
* <span data-ttu-id="44685-175">Om du vill använda vyn vertex körning finns [använda vyn Vertex körning i Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span><span class="sxs-lookup"><span data-stu-id="44685-175">To use the vertex execution view, see [Use the Vertex Execution View in Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span></span>
