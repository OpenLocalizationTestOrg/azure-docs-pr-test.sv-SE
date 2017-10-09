---
title: "aaaScale U-SQL lokalt köra och testa med Azure Data Lake U-SQL-SDK | Microsoft Docs"
description: "Lär dig hur toouse Azure Data Lake U-SQL-SDK tooscale U-SQL-jobb lokala körs och testa med kommandoraden och programmeringsgränssnitt på din lokala arbetsstationen."
services: data-lake-analytics
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: yanacai
ms.openlocfilehash: 2b0a16229789268e333f723ff6fc2c3efdc29905
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-u-sql-local-run-and-test-with-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="32b75-103">Skala lokal U-SQL-köra och testa med Azure Data Lake U-SQL-SDK</span><span class="sxs-lookup"><span data-stu-id="32b75-103">Scale U-SQL local run and test with Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="32b75-104">När du utvecklar U-SQL-skript, är det vanliga toorun och testar U-SQL-skript lokalt skicka in innan den toocloud.</span><span class="sxs-lookup"><span data-stu-id="32b75-104">When developing U-SQL script, it is common toorun and test U-SQL script locally before submit it toocloud.</span></span> <span data-ttu-id="32b75-105">Azure Data Lake innehåller en Nuget-paketet kallas SDK för Azure Data Lake U-SQL i det här scenariot, via som du sedan kan du skala lokal U-SQL kör och test.</span><span class="sxs-lookup"><span data-stu-id="32b75-105">Azure Data Lake provides a Nuget package called Azure Data Lake U-SQL SDK for this scenario, through which you can easily scale U-SQL local run and test.</span></span> <span data-ttu-id="32b75-106">Det är också möjligt toointegrate denna U-SQL testa med CI (kontinuerlig Integration) system tooautomate hello kompilera och test.</span><span class="sxs-lookup"><span data-stu-id="32b75-106">It is also possible toointegrate this U-SQL test with CI (Continuous Integration) system tooautomate hello compile and test.</span></span>

<span data-ttu-id="32b75-107">Om du bryr dig om hur toomanually lokala körs och felsöka U-SQL-skript med GUI-tooling kan du använda Azure Data Lake-verktyg för Visual Studio för den.</span><span class="sxs-lookup"><span data-stu-id="32b75-107">If you care about how toomanually local run and debug U-SQL script with GUI tooling, then you can use Azure Data Lake Tools for Visual Studio for that.</span></span> <span data-ttu-id="32b75-108">Mer information från [här](data-lake-analytics-data-lake-tools-local-run.md).</span><span class="sxs-lookup"><span data-stu-id="32b75-108">You can learn more from [here](data-lake-analytics-data-lake-tools-local-run.md).</span></span>

## <a name="install-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="32b75-109">Installera Azure Data Lake U-SQL-SDK</span><span class="sxs-lookup"><span data-stu-id="32b75-109">Install Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="32b75-110">Du kan hämta hello Azure Data Lake U-SQL-SDK [här](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) på Nuget.org. Och innan du använder den toomake att du har beroenden på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="32b75-110">You can get hello Azure Data Lake U-SQL SDK [here](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) on Nuget.org. And before using it, you need toomake sure you have dependencies as follows.</span></span>

### <a name="dependencies"></a><span data-ttu-id="32b75-111">Beroenden</span><span class="sxs-lookup"><span data-stu-id="32b75-111">Dependencies</span></span>

<span data-ttu-id="32b75-112">hello Data Lake U-SQL-SDK kräver hello följande beroenden:</span><span class="sxs-lookup"><span data-stu-id="32b75-112">hello Data Lake U-SQL SDK requires hello following dependencies:</span></span>

- <span data-ttu-id="32b75-113">[Microsoft .NET Framework 4.6 eller nyare](https://www.microsoft.com/download/details.aspx?id=17851).</span><span class="sxs-lookup"><span data-stu-id="32b75-113">[Microsoft .NET Framework 4.6 or newer](https://www.microsoft.com/download/details.aspx?id=17851).</span></span>
- <span data-ttu-id="32b75-114">Microsoft Visual C++-14 och Windows SDK 10.0.10240.0 eller senare (som kallas CppSDK i den här artikeln).</span><span class="sxs-lookup"><span data-stu-id="32b75-114">Microsoft Visual C++ 14 and Windows SDK 10.0.10240.0 or newer (which is called CppSDK in this article).</span></span> <span data-ttu-id="32b75-115">Det finns två sätt tooget CppSDK:</span><span class="sxs-lookup"><span data-stu-id="32b75-115">There are two ways tooget CppSDK:</span></span>

    - <span data-ttu-id="32b75-116">Installera [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou).</span><span class="sxs-lookup"><span data-stu-id="32b75-116">Install [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou).</span></span> <span data-ttu-id="32b75-117">Du har en \Windows Kits\10 mapp under hello mappen Program – till exempel C:\Program Files (x86) \Windows Kits\10\.</span><span class="sxs-lookup"><span data-stu-id="32b75-117">You'll have a \Windows Kits\10 folder under hello Program Files folder--for example, C:\Program Files (x86)\Windows Kits\10\.</span></span> <span data-ttu-id="32b75-118">Du hittar också hello SDK för Windows 10 version under \Windows Kits\10\Lib.</span><span class="sxs-lookup"><span data-stu-id="32b75-118">You'll also find hello Windows 10 SDK version under \Windows Kits\10\Lib.</span></span> <span data-ttu-id="32b75-119">Om du inte ser dessa mappar, installera om Visual Studio och vara säker på att tooselect hello SDK för Windows 10 under hello installationen.</span><span class="sxs-lookup"><span data-stu-id="32b75-119">If you don’t see these folders, reinstall Visual Studio and be sure tooselect hello Windows 10 SDK during hello installation.</span></span> <span data-ttu-id="32b75-120">Om du har det installeras med Visual Studio hittar lokala hello U-SQL-kompileraren den automatiskt.</span><span class="sxs-lookup"><span data-stu-id="32b75-120">If you have this installed with Visual Studio, hello U-SQL local compiler will find it automatically.</span></span>

    ![Data Lake-verktyg för Visual Studio lokala kör Windows 10-SDK](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

    - <span data-ttu-id="32b75-122">Installera [Data Lake-verktyg för Visual Studio](http://aka.ms/adltoolsvs).</span><span class="sxs-lookup"><span data-stu-id="32b75-122">Install [Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs).</span></span> <span data-ttu-id="32b75-123">Du kan hitta hello färdigförpackade Visual C++ och Windows SDK filer i C:\Program Files (x86) \Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK.</span><span class="sxs-lookup"><span data-stu-id="32b75-123">You can find hello prepackaged Visual C++ and Windows SDK files at C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK.</span></span> <span data-ttu-id="32b75-124">I det här fallet hittar lokala hello U-SQL-kompileraren inte hello beroenden automatiskt.</span><span class="sxs-lookup"><span data-stu-id="32b75-124">In this case, hello U-SQL local compiler cannot find hello dependencies automatically.</span></span> <span data-ttu-id="32b75-125">Du behöver toospecify hello CppSDK sökvägen för den.</span><span class="sxs-lookup"><span data-stu-id="32b75-125">You need toospecify hello CppSDK path for it.</span></span> <span data-ttu-id="32b75-126">Du kan kopiera hello tooanother platsen, eller så kan du använda den som den är.</span><span class="sxs-lookup"><span data-stu-id="32b75-126">You can either copy hello files tooanother location or use it as is.</span></span>

## <a name="understand-basic-concepts"></a><span data-ttu-id="32b75-127">Förstå grundläggande begrepp</span><span class="sxs-lookup"><span data-stu-id="32b75-127">Understand basic concepts</span></span>

### <a name="data-root"></a><span data-ttu-id="32b75-128">Dataroten</span><span class="sxs-lookup"><span data-stu-id="32b75-128">Data root</span></span>

<span data-ttu-id="32b75-129">hello data-rotmappen är ett ”lokalt Arkiv” för hello lokala beräkningskonto.</span><span class="sxs-lookup"><span data-stu-id="32b75-129">hello data-root folder is a "local store" for hello local compute account.</span></span> <span data-ttu-id="32b75-130">Det är likvärdiga toohello Azure Data Lake Store-konto för Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="32b75-130">It's equivalent toohello Azure Data Lake Store account of a Data Lake Analytics account.</span></span> <span data-ttu-id="32b75-131">Växla tooa olika data-rotmappen som är precis som växlar tooa olika store-konto.</span><span class="sxs-lookup"><span data-stu-id="32b75-131">Switching tooa different data-root folder is just like switching tooa different store account.</span></span> <span data-ttu-id="32b75-132">Om du vill tooaccess ofta delas data med olika dataroten mappar, måste du använda absoluta sökvägar i skript.</span><span class="sxs-lookup"><span data-stu-id="32b75-132">If you want tooaccess commonly shared data with different data-root folders, you must use absolute paths in your scripts.</span></span> <span data-ttu-id="32b75-133">Skapa filen system symboliska länkar (till exempel **mklink** på NTFS) under hello-dataroten mappen toopoint toohello delade data.</span><span class="sxs-lookup"><span data-stu-id="32b75-133">Or, create file system symbolic links (for example, **mklink** on NTFS) under hello data-root folder toopoint toohello shared data.</span></span>

<span data-ttu-id="32b75-134">hello data-rotmappen används för att:</span><span class="sxs-lookup"><span data-stu-id="32b75-134">hello data-root folder is used to:</span></span>

- <span data-ttu-id="32b75-135">Lagra lokala metadata, inklusive databaser, tabeller, tabellvärdesfunktioner (Tabellvärdesfunktioner) och sammansättningar.</span><span class="sxs-lookup"><span data-stu-id="32b75-135">Store local metadata, including databases, tables, table-valued functions (TVFs), and assemblies.</span></span>
- <span data-ttu-id="32b75-136">Leta upp hello indata och utdata sökvägar som har definierats som relativa sökvägar i U-SQL.</span><span class="sxs-lookup"><span data-stu-id="32b75-136">Look up hello input and output paths that are defined as relative paths in U-SQL.</span></span> <span data-ttu-id="32b75-137">Använda relativa sökvägar gör det enklare toodeploy tooAzure dina U-SQL-projekt.</span><span class="sxs-lookup"><span data-stu-id="32b75-137">Using relative paths makes it easier toodeploy your U-SQL projects tooAzure.</span></span>

### <a name="file-path-in-u-sql"></a><span data-ttu-id="32b75-138">Sökvägen i U-SQL</span><span class="sxs-lookup"><span data-stu-id="32b75-138">File path in U-SQL</span></span>

<span data-ttu-id="32b75-139">Du kan använda både en relativ sökväg och en lokal absolut sökväg i U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="32b75-139">You can use both a relative path and a local absolute path in U-SQL scripts.</span></span> <span data-ttu-id="32b75-140">hello relativ sökväg är relativ toohello angivna data-sökvägen till rotmappen.</span><span class="sxs-lookup"><span data-stu-id="32b75-140">hello relative path is relative toohello specified data-root folder path.</span></span> <span data-ttu-id="32b75-141">Vi rekommenderar att du använder ”/” som hello sökväg avgränsare toomake skripten som är kompatibel med hello på serversidan.</span><span class="sxs-lookup"><span data-stu-id="32b75-141">We recommend that you use "/" as hello path separator toomake your scripts compatible with hello server side.</span></span> <span data-ttu-id="32b75-142">Här följer några exempel på relativa sökvägar och deras motsvarande absoluta sökvägar.</span><span class="sxs-lookup"><span data-stu-id="32b75-142">Here are some examples of relative paths and their equivalent absolute paths.</span></span> <span data-ttu-id="32b75-143">I det här exemplet är C:\LocalRunDataRoot hello data-rotmappen.</span><span class="sxs-lookup"><span data-stu-id="32b75-143">In these examples, C:\LocalRunDataRoot is hello data-root folder.</span></span>

|<span data-ttu-id="32b75-144">Relativ sökväg</span><span class="sxs-lookup"><span data-stu-id="32b75-144">Relative path</span></span>|<span data-ttu-id="32b75-145">Absolut sökväg</span><span class="sxs-lookup"><span data-stu-id="32b75-145">Absolute path</span></span>|
|-------------|-------------|
|<span data-ttu-id="32b75-146">/ABC/def/Input.csv</span><span class="sxs-lookup"><span data-stu-id="32b75-146">/abc/def/input.csv</span></span> |<span data-ttu-id="32b75-147">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="32b75-147">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="32b75-148">ABC/def/Input.csv</span><span class="sxs-lookup"><span data-stu-id="32b75-148">abc/def/input.csv</span></span>  |<span data-ttu-id="32b75-149">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="32b75-149">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="32b75-150">D:/ABC/def/Input.csv</span><span class="sxs-lookup"><span data-stu-id="32b75-150">D:/abc/def/input.csv</span></span> |<span data-ttu-id="32b75-151">D:\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="32b75-151">D:\abc\def\input.csv</span></span>|

### <a name="working-directory"></a><span data-ttu-id="32b75-152">Arbetskatalog</span><span class="sxs-lookup"><span data-stu-id="32b75-152">Working directory</span></span>

<span data-ttu-id="32b75-153">När du kör hello U-SQL-skript lokalt, skapas en arbetskatalog under kompilering under katalogen körs.</span><span class="sxs-lookup"><span data-stu-id="32b75-153">When running hello U-SQL script locally, a working directory is created during compilation under current running directory.</span></span> <span data-ttu-id="32b75-154">Dessutom toohello kompilering utdata hello behövs runtime-filer för lokala körningen blir shadow kopierade toothis arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="32b75-154">In addition toohello compilation outputs, hello needed runtime files for local execution will be shadow copied toothis working directory.</span></span> <span data-ttu-id="32b75-155">hello working directory rotmappen kallas ”ScopeWorkDir” och hello filer under hello arbetskatalog är följande:</span><span class="sxs-lookup"><span data-stu-id="32b75-155">hello working directory root folder is called "ScopeWorkDir" and hello files under hello working directory are as follows:</span></span>

|<span data-ttu-id="32b75-156">Filen/katalogen</span><span class="sxs-lookup"><span data-stu-id="32b75-156">Directory/file</span></span>|<span data-ttu-id="32b75-157">Filen/katalogen</span><span class="sxs-lookup"><span data-stu-id="32b75-157">Directory/file</span></span>|<span data-ttu-id="32b75-158">Filen/katalogen</span><span class="sxs-lookup"><span data-stu-id="32b75-158">Directory/file</span></span>|<span data-ttu-id="32b75-159">Definition</span><span class="sxs-lookup"><span data-stu-id="32b75-159">Definition</span></span>|<span data-ttu-id="32b75-160">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="32b75-160">Description</span></span>|
|--------------|--------------|--------------|----------|-----------|
|<span data-ttu-id="32b75-161">C6A101DDCB470506</span><span class="sxs-lookup"><span data-stu-id="32b75-161">C6A101DDCB470506</span></span>| | |<span data-ttu-id="32b75-162">Hash-sträng av körningsversion</span><span class="sxs-lookup"><span data-stu-id="32b75-162">Hash string of runtime version</span></span>|<span data-ttu-id="32b75-163">Skuggkopia av runtime-filer som behövs för lokal körning</span><span class="sxs-lookup"><span data-stu-id="32b75-163">Shadow copy of runtime files needed for local execution</span></span>|
| |<span data-ttu-id="32b75-164">Script_66AE4909AA0ED06C</span><span class="sxs-lookup"><span data-stu-id="32b75-164">Script_66AE4909AA0ED06C</span></span>| |<span data-ttu-id="32b75-165">Script namn + hash-sträng med sökvägen för skriptet</span><span class="sxs-lookup"><span data-stu-id="32b75-165">Script name + hash string of script path</span></span>|<span data-ttu-id="32b75-166">Utdata för kompilering och körning steg loggning</span><span class="sxs-lookup"><span data-stu-id="32b75-166">Compilation outputs and execution step logging</span></span>|
| | |<span data-ttu-id="32b75-167">\_skriptet\_.abr</span><span class="sxs-lookup"><span data-stu-id="32b75-167">\_script\_.abr</span></span>|<span data-ttu-id="32b75-168">Utdata för kompilator</span><span class="sxs-lookup"><span data-stu-id="32b75-168">Compiler output</span></span>|<span data-ttu-id="32b75-169">Algebra fil</span><span class="sxs-lookup"><span data-stu-id="32b75-169">Algebra file</span></span>|
| | |<span data-ttu-id="32b75-170">\_ScopeCodeGen\_. *</span><span class="sxs-lookup"><span data-stu-id="32b75-170">\_ScopeCodeGen\_.*</span></span>|<span data-ttu-id="32b75-171">Utdata för kompilator</span><span class="sxs-lookup"><span data-stu-id="32b75-171">Compiler output</span></span>|<span data-ttu-id="32b75-172">Genererade hanterad kod</span><span class="sxs-lookup"><span data-stu-id="32b75-172">Generated managed code</span></span>|
| | |<span data-ttu-id="32b75-173">\_ScopeCodeGenEngine\_. *</span><span class="sxs-lookup"><span data-stu-id="32b75-173">\_ScopeCodeGenEngine\_.*</span></span>|<span data-ttu-id="32b75-174">Utdata för kompilator</span><span class="sxs-lookup"><span data-stu-id="32b75-174">Compiler output</span></span>|<span data-ttu-id="32b75-175">Genererade maskinkod</span><span class="sxs-lookup"><span data-stu-id="32b75-175">Generated native code</span></span>|
| | |<span data-ttu-id="32b75-176">Refererade sammansättningar</span><span class="sxs-lookup"><span data-stu-id="32b75-176">referenced assemblies</span></span>|<span data-ttu-id="32b75-177">Sammansättningsreferensen</span><span class="sxs-lookup"><span data-stu-id="32b75-177">Assembly reference</span></span>|<span data-ttu-id="32b75-178">Sammansättningsfiler</span><span class="sxs-lookup"><span data-stu-id="32b75-178">Referenced assembly files</span></span>|
| | |<span data-ttu-id="32b75-179">deployed_resources</span><span class="sxs-lookup"><span data-stu-id="32b75-179">deployed_resources</span></span>|<span data-ttu-id="32b75-180">Resurs-distribution</span><span class="sxs-lookup"><span data-stu-id="32b75-180">Resource deployment</span></span>|<span data-ttu-id="32b75-181">Resursfiler för distribution</span><span class="sxs-lookup"><span data-stu-id="32b75-181">Resource deployment files</span></span>|
| | |<span data-ttu-id="32b75-182">xxxxxxxx.xxx[1..n]\_\*. *</span><span class="sxs-lookup"><span data-stu-id="32b75-182">xxxxxxxx.xxx[1..n]\_\*.*</span></span>|<span data-ttu-id="32b75-183">Körningsloggen</span><span class="sxs-lookup"><span data-stu-id="32b75-183">Execution log</span></span>|<span data-ttu-id="32b75-184">Logg över utförande</span><span class="sxs-lookup"><span data-stu-id="32b75-184">Log of execution steps</span></span>|


## <a name="use-hello-sdk-from-hello-command-line"></a><span data-ttu-id="32b75-185">Använd hello SDK från hello kommandorad</span><span class="sxs-lookup"><span data-stu-id="32b75-185">Use hello SDK from hello command line</span></span>

### <a name="command-line-interface-of-hello-helper-application"></a><span data-ttu-id="32b75-186">Kommandoradsgränssnitt för hello hjälpprogram</span><span class="sxs-lookup"><span data-stu-id="32b75-186">Command-line interface of hello helper application</span></span>

<span data-ttu-id="32b75-187">Under SDK directory\build\runtime är LocalRunHelper.exe hello kommandoradsverktyget hjälpprogram som innehåller gränssnitt toomost hello används vanligtvis för lokal kör funktioner.</span><span class="sxs-lookup"><span data-stu-id="32b75-187">Under SDK directory\build\runtime, LocalRunHelper.exe is hello command-line helper application that provides interfaces toomost of hello commonly used local-run functions.</span></span> <span data-ttu-id="32b75-188">Observera att både hello kommandot och hello argumentväxlar är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="32b75-188">Note that both hello command and hello argument switches are case-sensitive.</span></span> <span data-ttu-id="32b75-189">tooinvoke den:</span><span class="sxs-lookup"><span data-stu-id="32b75-189">tooinvoke it:</span></span>

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

<span data-ttu-id="32b75-190">Kör LocalRunHelper.exe utan argument eller med hello **hjälp** växla tooshow hello hjälpinformation:</span><span class="sxs-lookup"><span data-stu-id="32b75-190">Run LocalRunHelper.exe without arguments or with hello **help** switch tooshow hello help information:</span></span>

    > LocalRunHelper.exe help

        Command 'help' :  Show usage information
        Command 'compile' :  Compile hello script
        Required Arguments :
            -Script param
                    Script File Path
        Optional Arguments :
            -Shallow [default value 'False']
                    Shallow compile

<span data-ttu-id="32b75-191">I hello hjälpinformation:</span><span class="sxs-lookup"><span data-stu-id="32b75-191">In hello help information:</span></span>

-  <span data-ttu-id="32b75-192">**Kommandot** ger hello kommandots namn.</span><span class="sxs-lookup"><span data-stu-id="32b75-192">**Command** gives hello command’s name.</span></span>  
-  <span data-ttu-id="32b75-193">**Argumentet är obligatoriskt** listas argument måste anges.</span><span class="sxs-lookup"><span data-stu-id="32b75-193">**Required Argument** lists arguments that must be supplied.</span></span>  
-  <span data-ttu-id="32b75-194">**Valfria argumentet** listas argument som är valfria med standardvärden.</span><span class="sxs-lookup"><span data-stu-id="32b75-194">**Optional Argument** lists arguments that are optional, with default values.</span></span>  <span data-ttu-id="32b75-195">Valfri boolesk argument ha inte parametrar, och deras utseende innebär negativt tootheir standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="32b75-195">Optional Boolean arguments don’t have parameters, and their appearances mean negative tootheir default value.</span></span>

### <a name="return-value-and-logging"></a><span data-ttu-id="32b75-196">Returvärdet och loggning</span><span class="sxs-lookup"><span data-stu-id="32b75-196">Return value and logging</span></span>

<span data-ttu-id="32b75-197">hello hjälpprogram returnerar **0** för lyckad och **-1** för felet.</span><span class="sxs-lookup"><span data-stu-id="32b75-197">hello helper application returns **0** for success and **-1** for failure.</span></span> <span data-ttu-id="32b75-198">Som standard skickar hello helper alla meddelanden toohello aktuella konsolen.</span><span class="sxs-lookup"><span data-stu-id="32b75-198">By default, hello helper sends all messages toohello current console.</span></span> <span data-ttu-id="32b75-199">De flesta av hello kommandon stöder dock hello **- MessageOut sökväg_till_loggfil** valfritt argument som omdirigerar hello matar ut tooa loggfilen.</span><span class="sxs-lookup"><span data-stu-id="32b75-199">However, most of hello commands support hello **-MessageOut path_to_log_file** optional argument that redirects hello outputs tooa log file.</span></span>

### <a name="environment-variable-configuring"></a><span data-ttu-id="32b75-200">Miljö variabeln konfigurera</span><span class="sxs-lookup"><span data-stu-id="32b75-200">Environment variable configuring</span></span>

<span data-ttu-id="32b75-201">Lokal U-SQL kör behov angivna data från en rotcertifikatutfärdare som konto för lokal lagring, samt en angiven CppSDK sökväg för beroenden.</span><span class="sxs-lookup"><span data-stu-id="32b75-201">U-SQL local run needs a specified data root as local storage account, as well as a specified CppSDK path for dependencies.</span></span> <span data-ttu-id="32b75-202">Du kan både set hello-argumentet i kommandorads- eller ange miljövariabeln för dessa.</span><span class="sxs-lookup"><span data-stu-id="32b75-202">You can both set hello argument in command-line or set environment variable for them.</span></span>

- <span data-ttu-id="32b75-203">Ange hello **SCOPE_CPP_SDK** miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="32b75-203">Set hello **SCOPE_CPP_SDK** environment variable.</span></span>

    <span data-ttu-id="32b75-204">Om du får Microsoft Visual C++ och hello Windows SDK genom att installera Data Lake-verktyg för Visual Studio kan du kontrollera att du har hello följande mapp:</span><span class="sxs-lookup"><span data-stu-id="32b75-204">If you get Microsoft Visual C++ and hello Windows SDK by installing Data Lake Tools for Visual Studio, verify that you have hello following folder:</span></span>

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    <span data-ttu-id="32b75-205">Definiera en ny miljövariabel som heter **SCOPE_CPP_SDK** toopoint toothis directory.</span><span class="sxs-lookup"><span data-stu-id="32b75-205">Define a new environment variable called **SCOPE_CPP_SDK** toopoint toothis directory.</span></span> <span data-ttu-id="32b75-206">Kopiera hello mappen toohello annan plats och ange **SCOPE_CPP_SDK** som.</span><span class="sxs-lookup"><span data-stu-id="32b75-206">Or copy hello folder toohello other location and specify **SCOPE_CPP_SDK** as that.</span></span>

    <span data-ttu-id="32b75-207">Du kan ange hello i miljövariabeln tillägg toosetting hello, **- CppSDK** argumentet när du använder hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="32b75-207">In addition toosetting hello environment variable, you can specify hello **-CppSDK** argument when you're using hello command line.</span></span> <span data-ttu-id="32b75-208">Det här argumentet skriver över dina standard CppSDK miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="32b75-208">This argument overwrites your default CppSDK environment variable.</span></span>

- <span data-ttu-id="32b75-209">Ange hello **LOCALRUN_DATAROOT** miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="32b75-209">Set hello **LOCALRUN_DATAROOT** environment variable.</span></span>

    <span data-ttu-id="32b75-210">Definiera en ny miljövariabel som heter **LOCALRUN_DATAROOT** som pekar toohello data rot.</span><span class="sxs-lookup"><span data-stu-id="32b75-210">Define a new environment variable called **LOCALRUN_DATAROOT** that points toohello data root.</span></span>

    <span data-ttu-id="32b75-211">Du kan ange hello i miljövariabeln tillägg toosetting hello, **- DataRoot** argumentet med hello data-rotsökvägen när du använder en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="32b75-211">In addition toosetting hello environment variable, you can specify hello **-DataRoot** argument with hello data-root path when you're using a command line.</span></span> <span data-ttu-id="32b75-212">Det här argumentet skriver över dina standard dataroten miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="32b75-212">This argument overwrites your default data-root environment variable.</span></span> <span data-ttu-id="32b75-213">Du måste tooadd argumentet tooevery kommandoraden du kör så att du kan skriva över standard hello-dataroten miljövariabeln för alla åtgärder.</span><span class="sxs-lookup"><span data-stu-id="32b75-213">You need tooadd this argument tooevery command line you're running so that you can overwrite hello default data-root environment variable for all operations.</span></span>

### <a name="sdk-command-line-usage-samples"></a><span data-ttu-id="32b75-214">SDK kommandoraden användning prover</span><span class="sxs-lookup"><span data-stu-id="32b75-214">SDK command line usage samples</span></span>

#### <a name="compile-and-run"></a><span data-ttu-id="32b75-215">Kompilera och kör</span><span class="sxs-lookup"><span data-stu-id="32b75-215">Compile and run</span></span>

<span data-ttu-id="32b75-216">Hej **kör** kommandot används toocompile hello skript och köra kompilerade resultatet.</span><span class="sxs-lookup"><span data-stu-id="32b75-216">hello **run** command is used toocompile hello script and then execute compiled results.</span></span> <span data-ttu-id="32b75-217">Dess kommandoradsargument är en kombination av dessa från **Kompilera** och **köra**.</span><span class="sxs-lookup"><span data-stu-id="32b75-217">Its command-line arguments are a combination of those from **compile** and **execute**.</span></span>

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

<span data-ttu-id="32b75-218">hello följande är valfria argument för **kör**:</span><span class="sxs-lookup"><span data-stu-id="32b75-218">hello following are optional arguments for **run**:</span></span>


|<span data-ttu-id="32b75-219">Argumentet</span><span class="sxs-lookup"><span data-stu-id="32b75-219">Argument</span></span>|<span data-ttu-id="32b75-220">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="32b75-220">Default value</span></span>|<span data-ttu-id="32b75-221">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="32b75-221">Description</span></span>|
|--------|-------------|-----------|
|<span data-ttu-id="32b75-222">-Bakomliggande koden</span><span class="sxs-lookup"><span data-stu-id="32b75-222">-CodeBehind</span></span>|<span data-ttu-id="32b75-223">False</span><span class="sxs-lookup"><span data-stu-id="32b75-223">False</span></span>|<span data-ttu-id="32b75-224">hello skriptet har .cs koden bakom</span><span class="sxs-lookup"><span data-stu-id="32b75-224">hello script has .cs code behind</span></span>|
|<span data-ttu-id="32b75-225">-CppSDK</span><span class="sxs-lookup"><span data-stu-id="32b75-225">-CppSDK</span></span>| |<span data-ttu-id="32b75-226">CppSDK Directory</span><span class="sxs-lookup"><span data-stu-id="32b75-226">CppSDK Directory</span></span>|
|<span data-ttu-id="32b75-227">-DataRoot</span><span class="sxs-lookup"><span data-stu-id="32b75-227">-DataRoot</span></span>| <span data-ttu-id="32b75-228">DataRoot miljövariabel</span><span class="sxs-lookup"><span data-stu-id="32b75-228">DataRoot environment variable</span></span>|<span data-ttu-id="32b75-229">DataRoot för lokala kör standard för miljövariabeln 'LOCALRUN_DATAROOT'</span><span class="sxs-lookup"><span data-stu-id="32b75-229">DataRoot for local run, default too'LOCALRUN_DATAROOT' environment variable</span></span>|
|<span data-ttu-id="32b75-230">-MessageOut</span><span class="sxs-lookup"><span data-stu-id="32b75-230">-MessageOut</span></span>| |<span data-ttu-id="32b75-231">Dump meddelanden på konsolen tooa fil</span><span class="sxs-lookup"><span data-stu-id="32b75-231">Dump messages on console tooa file</span></span>|
|<span data-ttu-id="32b75-232">-Parallell</span><span class="sxs-lookup"><span data-stu-id="32b75-232">-Parallel</span></span>|<span data-ttu-id="32b75-233">1</span><span class="sxs-lookup"><span data-stu-id="32b75-233">1</span></span>|<span data-ttu-id="32b75-234">Kör hello plan med hello angetts parallellitet</span><span class="sxs-lookup"><span data-stu-id="32b75-234">Run hello plan with hello specified parallelism</span></span>|
|<span data-ttu-id="32b75-235">-Referenser</span><span class="sxs-lookup"><span data-stu-id="32b75-235">-References</span></span>| |<span data-ttu-id="32b75-236">Lista över sökvägar tooextra referenssammansättningar eller datafiler i koden bakom, avgränsade med ';'</span><span class="sxs-lookup"><span data-stu-id="32b75-236">List of paths tooextra reference assemblies or data files of code behind, separated by ';'</span></span>|
|<span data-ttu-id="32b75-237">-UdoRedirect</span><span class="sxs-lookup"><span data-stu-id="32b75-237">-UdoRedirect</span></span>|<span data-ttu-id="32b75-238">False</span><span class="sxs-lookup"><span data-stu-id="32b75-238">False</span></span>|<span data-ttu-id="32b75-239">Generera Udo sammansättningen omdirigering config</span><span class="sxs-lookup"><span data-stu-id="32b75-239">Generate Udo assembly redirect config</span></span>|
|<span data-ttu-id="32b75-240">-UseDatabase</span><span class="sxs-lookup"><span data-stu-id="32b75-240">-UseDatabase</span></span>|<span data-ttu-id="32b75-241">master</span><span class="sxs-lookup"><span data-stu-id="32b75-241">master</span></span>|<span data-ttu-id="32b75-242">Databasen toouse för koden bakom tillfällig sammansättning registrering</span><span class="sxs-lookup"><span data-stu-id="32b75-242">Database toouse for code behind temporary assembly registration</span></span>|
|<span data-ttu-id="32b75-243">-Verbose</span><span class="sxs-lookup"><span data-stu-id="32b75-243">-Verbose</span></span>|<span data-ttu-id="32b75-244">False</span><span class="sxs-lookup"><span data-stu-id="32b75-244">False</span></span>|<span data-ttu-id="32b75-245">Visa detaljerade utdata från runtime</span><span class="sxs-lookup"><span data-stu-id="32b75-245">Show detailed outputs from runtime</span></span>|
|<span data-ttu-id="32b75-246">-WorkDir</span><span class="sxs-lookup"><span data-stu-id="32b75-246">-WorkDir</span></span>|<span data-ttu-id="32b75-247">Aktuell katalog</span><span class="sxs-lookup"><span data-stu-id="32b75-247">Current Directory</span></span>|<span data-ttu-id="32b75-248">Katalogen för kompileraren användnings- och utdata</span><span class="sxs-lookup"><span data-stu-id="32b75-248">Directory for compiler usage and outputs</span></span>|
|<span data-ttu-id="32b75-249">-RunScopeCEP</span><span class="sxs-lookup"><span data-stu-id="32b75-249">-RunScopeCEP</span></span>|<span data-ttu-id="32b75-250">0</span><span class="sxs-lookup"><span data-stu-id="32b75-250">0</span></span>|<span data-ttu-id="32b75-251">ScopeCEP läge toouse</span><span class="sxs-lookup"><span data-stu-id="32b75-251">ScopeCEP mode toouse</span></span>|
|<span data-ttu-id="32b75-252">-ScopeCEPTempPath</span><span class="sxs-lookup"><span data-stu-id="32b75-252">-ScopeCEPTempPath</span></span>|<span data-ttu-id="32b75-253">Temp</span><span class="sxs-lookup"><span data-stu-id="32b75-253">temp</span></span>|<span data-ttu-id="32b75-254">Sökvägen toouse för strömmande data</span><span class="sxs-lookup"><span data-stu-id="32b75-254">Temp path toouse for streaming data</span></span>|
|<span data-ttu-id="32b75-255">-OptFlags</span><span class="sxs-lookup"><span data-stu-id="32b75-255">-OptFlags</span></span>| |<span data-ttu-id="32b75-256">Kommaavgränsad lista över optimering flaggor</span><span class="sxs-lookup"><span data-stu-id="32b75-256">Comma-separated list of optimizer flags</span></span>|


<span data-ttu-id="32b75-257">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="32b75-257">Here's an example:</span></span>

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

<span data-ttu-id="32b75-258">Förutom att kombinera **Kompilera** och **köra**, kan du kompilera och köra körbara filer hello kompileras separat.</span><span class="sxs-lookup"><span data-stu-id="32b75-258">Besides combining **compile** and **execute**, you can compile and execute hello compiled executables separately.</span></span>

#### <a name="compile-a-u-sql-script"></a><span data-ttu-id="32b75-259">Kompilera ett U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="32b75-259">Compile a U-SQL script</span></span>

<span data-ttu-id="32b75-260">Hej **Kompilera** kommandot är tooexecutables för används toocompile U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="32b75-260">hello **compile** command is used toocompile a U-SQL script tooexecutables.</span></span>

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

<span data-ttu-id="32b75-261">hello följande är valfria argument för **Kompilera**:</span><span class="sxs-lookup"><span data-stu-id="32b75-261">hello following are optional arguments for **compile**:</span></span>


|<span data-ttu-id="32b75-262">Argumentet</span><span class="sxs-lookup"><span data-stu-id="32b75-262">Argument</span></span>|<span data-ttu-id="32b75-263">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="32b75-263">Description</span></span>|
|--------|-----------|
| <span data-ttu-id="32b75-264">-Bakomliggande koden [standardvärdet 'False']</span><span class="sxs-lookup"><span data-stu-id="32b75-264">-CodeBehind [default value 'False']</span></span>|<span data-ttu-id="32b75-265">hello skriptet har .cs koden bakom</span><span class="sxs-lookup"><span data-stu-id="32b75-265">hello script has .cs code behind</span></span>|
| <span data-ttu-id="32b75-266">-CppSDK [standardvärde '']</span><span class="sxs-lookup"><span data-stu-id="32b75-266">-CppSDK [default value '']</span></span>|<span data-ttu-id="32b75-267">CppSDK Directory</span><span class="sxs-lookup"><span data-stu-id="32b75-267">CppSDK Directory</span></span>|
| <span data-ttu-id="32b75-268">-DataRoot [standardvärdet 'DataRoot miljövariabeln']</span><span class="sxs-lookup"><span data-stu-id="32b75-268">-DataRoot [default value 'DataRoot environment variable']</span></span>|<span data-ttu-id="32b75-269">DataRoot för lokala kör standard för miljövariabeln 'LOCALRUN_DATAROOT'</span><span class="sxs-lookup"><span data-stu-id="32b75-269">DataRoot for local run, default too'LOCALRUN_DATAROOT' environment variable</span></span>|
| <span data-ttu-id="32b75-270">-MessageOut [standardvärde '']</span><span class="sxs-lookup"><span data-stu-id="32b75-270">-MessageOut [default value '']</span></span>|<span data-ttu-id="32b75-271">Dump meddelanden på konsolen tooa fil</span><span class="sxs-lookup"><span data-stu-id="32b75-271">Dump messages on console tooa file</span></span>|
| <span data-ttu-id="32b75-272">-Hänvisar till [standardvärde '']</span><span class="sxs-lookup"><span data-stu-id="32b75-272">-References [default value '']</span></span>|<span data-ttu-id="32b75-273">Lista över sökvägar tooextra referenssammansättningar eller datafiler i koden bakom, avgränsade med ';'</span><span class="sxs-lookup"><span data-stu-id="32b75-273">List of paths tooextra reference assemblies or data files of code behind, separated by ';'</span></span>|
| <span data-ttu-id="32b75-274">-Kort [standardvärdet 'False']</span><span class="sxs-lookup"><span data-stu-id="32b75-274">-Shallow [default value 'False']</span></span>|<span data-ttu-id="32b75-275">Lite kompilera</span><span class="sxs-lookup"><span data-stu-id="32b75-275">Shallow compile</span></span>|
| <span data-ttu-id="32b75-276">-UdoRedirect [standardvärdet 'False']</span><span class="sxs-lookup"><span data-stu-id="32b75-276">-UdoRedirect [default value 'False']</span></span>|<span data-ttu-id="32b75-277">Generera Udo sammansättningen omdirigering config</span><span class="sxs-lookup"><span data-stu-id="32b75-277">Generate Udo assembly redirect config</span></span>|
| <span data-ttu-id="32b75-278">-UseDatabase [standardvärdet 'master']</span><span class="sxs-lookup"><span data-stu-id="32b75-278">-UseDatabase [default value 'master']</span></span>|<span data-ttu-id="32b75-279">Databasen toouse för koden bakom tillfällig sammansättning registrering</span><span class="sxs-lookup"><span data-stu-id="32b75-279">Database toouse for code behind temporary assembly registration</span></span>|
| <span data-ttu-id="32b75-280">-WorkDir [standardvärdet 'Katalogen']</span><span class="sxs-lookup"><span data-stu-id="32b75-280">-WorkDir [default value 'Current Directory']</span></span>|<span data-ttu-id="32b75-281">Katalogen för kompileraren användnings- och utdata</span><span class="sxs-lookup"><span data-stu-id="32b75-281">Directory for compiler usage and outputs</span></span>|
| <span data-ttu-id="32b75-282">-RunScopeCEP [standardvärdet '0']</span><span class="sxs-lookup"><span data-stu-id="32b75-282">-RunScopeCEP [default value '0']</span></span>|<span data-ttu-id="32b75-283">ScopeCEP läge toouse</span><span class="sxs-lookup"><span data-stu-id="32b75-283">ScopeCEP mode toouse</span></span>|
| <span data-ttu-id="32b75-284">-ScopeCEPTempPath [standardvärdet 'temp']</span><span class="sxs-lookup"><span data-stu-id="32b75-284">-ScopeCEPTempPath [default value 'temp']</span></span>|<span data-ttu-id="32b75-285">Sökvägen toouse för strömmande data</span><span class="sxs-lookup"><span data-stu-id="32b75-285">Temp path toouse for streaming data</span></span>|
| <span data-ttu-id="32b75-286">-OptFlags [standardvärde '']</span><span class="sxs-lookup"><span data-stu-id="32b75-286">-OptFlags [default value '']</span></span>|<span data-ttu-id="32b75-287">Kommaavgränsad lista över optimering flaggor</span><span class="sxs-lookup"><span data-stu-id="32b75-287">Comma-separated list of optimizer flags</span></span>|


<span data-ttu-id="32b75-288">Här följer några exempel på användning.</span><span class="sxs-lookup"><span data-stu-id="32b75-288">Here are some usage examples.</span></span>

<span data-ttu-id="32b75-289">Kompilera ett U-SQL-skript:</span><span class="sxs-lookup"><span data-stu-id="32b75-289">Compile a U-SQL script:</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql

<span data-ttu-id="32b75-290">Kompilera ett U-SQL-skript och ange hello data-rotmappen.</span><span class="sxs-lookup"><span data-stu-id="32b75-290">Compile a U-SQL script and set hello data-root folder.</span></span> <span data-ttu-id="32b75-291">Observera att detta ersätter hello set-miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="32b75-291">Note that this will overwrite hello set environment variable.</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

<span data-ttu-id="32b75-292">Kompilera ett U-SQL-skript och ange en arbetskatalog och referenssammansättning databasen:</span><span class="sxs-lookup"><span data-stu-id="32b75-292">Compile a U-SQL script and set a working directory, reference assembly, and database:</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a><span data-ttu-id="32b75-293">Köra kompilerade resultat</span><span class="sxs-lookup"><span data-stu-id="32b75-293">Execute compiled results</span></span>

<span data-ttu-id="32b75-294">Hej **köra** kommandot är används tooexecute kompileras resultat.</span><span class="sxs-lookup"><span data-stu-id="32b75-294">hello **execute** command is used tooexecute compiled results.</span></span>   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

<span data-ttu-id="32b75-295">hello följande är valfria argument för **köra**:</span><span class="sxs-lookup"><span data-stu-id="32b75-295">hello following are optional arguments for **execute**:</span></span>

|<span data-ttu-id="32b75-296">Argumentet</span><span class="sxs-lookup"><span data-stu-id="32b75-296">Argument</span></span>|<span data-ttu-id="32b75-297">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="32b75-297">Description</span></span>|
|--------|-----------|
|<span data-ttu-id="32b75-298">-DataRoot [standardvärde '']</span><span class="sxs-lookup"><span data-stu-id="32b75-298">-DataRoot [default value '']</span></span>|<span data-ttu-id="32b75-299">Dataroten för körning av metadata.</span><span class="sxs-lookup"><span data-stu-id="32b75-299">Data root for metadata execution.</span></span> <span data-ttu-id="32b75-300">Standard toohello **LOCALRUN_DATAROOT** miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="32b75-300">It defaults toohello **LOCALRUN_DATAROOT** environment variable.</span></span>|
|<span data-ttu-id="32b75-301">-MessageOut [standardvärde '']</span><span class="sxs-lookup"><span data-stu-id="32b75-301">-MessageOut [default value '']</span></span>|<span data-ttu-id="32b75-302">Dumpa meddelanden på hello tooa-fil.</span><span class="sxs-lookup"><span data-stu-id="32b75-302">Dump messages on hello console tooa file.</span></span>|
|<span data-ttu-id="32b75-303">-Parallell [standardvärdet '1']</span><span class="sxs-lookup"><span data-stu-id="32b75-303">-Parallel [default value '1']</span></span>|<span data-ttu-id="32b75-304">Indikator toorun hello genereras lokala kör steg med hello angetts parallellitet nivå.</span><span class="sxs-lookup"><span data-stu-id="32b75-304">Indicator toorun hello generated local-run steps with hello specified parallelism level.</span></span>|
|<span data-ttu-id="32b75-305">-Verbose [standardvärde 'False']</span><span class="sxs-lookup"><span data-stu-id="32b75-305">-Verbose [default value 'False']</span></span>|<span data-ttu-id="32b75-306">Indikator tooshow detaljerade utdata från körning.</span><span class="sxs-lookup"><span data-stu-id="32b75-306">Indicator tooshow detailed outputs from runtime.</span></span>|

<span data-ttu-id="32b75-307">Här är ett exempel på användning:</span><span class="sxs-lookup"><span data-stu-id="32b75-307">Here's a usage example:</span></span>

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-hello-sdk-with-programming-interfaces"></a><span data-ttu-id="32b75-308">Använda hello SDK med programmeringsgränssnitt</span><span class="sxs-lookup"><span data-stu-id="32b75-308">Use hello SDK with programming interfaces</span></span>

<span data-ttu-id="32b75-309">hello programmeringsgränssnitt finns i hello LocalRunHelper.exe.</span><span class="sxs-lookup"><span data-stu-id="32b75-309">hello programming interfaces are all located in hello LocalRunHelper.exe.</span></span> <span data-ttu-id="32b75-310">Du kan använda dem toointegrate hello funktioner för hello U-SQL-SDK och hello C# test framework tooscale U-SQL-skript lokalt testet.</span><span class="sxs-lookup"><span data-stu-id="32b75-310">You can use them toointegrate hello functionality of hello U-SQL SDK and hello C# test framework tooscale your U-SQL script local test.</span></span> <span data-ttu-id="32b75-311">I den här artikeln ska jag använda hello standard C# enhet test projektet tooshow hur toouse dessa gränssnitt tootest U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="32b75-311">In this article, I will use hello standard C# unit test project tooshow how toouse these interfaces tootest your U-SQL script.</span></span>

### <a name="step-1-create-c-unit-test-project-and-configuration"></a><span data-ttu-id="32b75-312">Steg 1: Skapa C# enhet test-projekt och konfiguration</span><span class="sxs-lookup"><span data-stu-id="32b75-312">Step 1: Create C# unit test project and configuration</span></span>

- <span data-ttu-id="32b75-313">Skapa ett C# enhet testprojekt via fil > Nytt > Projekt > Visual C# > Testa > enhet Testa projektet.</span><span class="sxs-lookup"><span data-stu-id="32b75-313">Create a C# unit test project through File > New > Project > Visual C# > Test > Unit Test Project.</span></span>
- <span data-ttu-id="32b75-314">Lägg till LocalRunHelper.exe som en referens för hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="32b75-314">Add LocalRunHelper.exe as a reference for hello project.</span></span> <span data-ttu-id="32b75-315">Hej LocalRunHelper.exe finns i \build\runtime\LocalRunHelper.exe i Nuget-paketet.</span><span class="sxs-lookup"><span data-stu-id="32b75-315">hello LocalRunHelper.exe is located at \build\runtime\LocalRunHelper.exe in Nuget package.</span></span>

    ![Azure Data Lake U-SQL-SDK Lägg till referens](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- <span data-ttu-id="32b75-317">U-SQL-SDK **endast** x64 Stödmiljö, se till att tooset build platform mål som x64.</span><span class="sxs-lookup"><span data-stu-id="32b75-317">U-SQL SDK **only** support x64 environment, make sure tooset build platform target as x64.</span></span> <span data-ttu-id="32b75-318">Du kan ange som via projektegenskapen > Skapa > plattform mål.</span><span class="sxs-lookup"><span data-stu-id="32b75-318">You can set that through Project Property > Build > Platform target.</span></span>

    ![Azure Data Lake U-SQL-SDK konfigurera x64 projekt](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- <span data-ttu-id="32b75-320">Se till att tooset din testmiljö som x64.</span><span class="sxs-lookup"><span data-stu-id="32b75-320">Make sure tooset your test environment as x64.</span></span> <span data-ttu-id="32b75-321">I Visual Studio kan du ändra det genom testning > Testa inställningar > standard processorarkitektur > x64.</span><span class="sxs-lookup"><span data-stu-id="32b75-321">In Visual Studio, you can set it through Test > Test Settings > Default Processor Architecture > x64.</span></span>

    ![Azure Data Lake U-SQL-SDK konfigurera x64 testmiljö](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- <span data-ttu-id="32b75-323">Se till att toocopy alla beroende filer under NugetPackage\build\runtime\ tooproject arbetskatalogen som vanligtvis är under ProjectFolder\bin\x64\Debug.</span><span class="sxs-lookup"><span data-stu-id="32b75-323">Make sure toocopy all dependency files under NugetPackage\build\runtime\ tooproject working directory which is usually under ProjectFolder\bin\x64\Debug.</span></span>

### <a name="step-2-create-u-sql-script-test-case"></a><span data-ttu-id="32b75-324">Steg 2: Skapa testfall för U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="32b75-324">Step 2: Create U-SQL script test case</span></span>

<span data-ttu-id="32b75-325">Nedan visas hello exempelkod för U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="32b75-325">Below is hello sample code for U-SQL script test.</span></span> <span data-ttu-id="32b75-326">För att testa, behöver du tooprepare skript indata och utdata som förväntas-filer.</span><span class="sxs-lookup"><span data-stu-id="32b75-326">For testing, you need tooprepare scripts, input files and expected output files.</span></span>

    using System;
    using Microsoft.VisualStudio.TestTools.UnitTesting;
    using System.IO;
    using System.Text;
    using System.Security.Cryptography;
    using Microsoft.Analytics.LocalRun;

    namespace UnitTestProject1
    {
        [TestClass]
        public class USQLUnitTest
        {
            [TestMethod]
            public void TestUSQLScript()
            {
                //Specify hello local run message output path
                StreamWriter MessageOutput = new StreamWriter("../../../log.txt");

                LocalRunHelper localrun = new LocalRunHelper(MessageOutput);

                //Configure hello DateRoot path, Script Path and CPPSDK path
                localrun.DataRoot = "../../../";
                localrun.ScriptPath = "../../../Script/Script.usql";
                localrun.CppSdkDir = "../../../CppSDK";

                //Run U-SQL script
                localrun.DoRun();

                //Script output 
                string Result = Path.Combine(localrun.DataRoot, "Output/result.csv");

                //Expected script output
                string ExpectedResult = "../../../ExpectedOutput/result.csv";

                Test.Helpers.FileAssert.AreEqual(Result, ExpectedResult);

                //Don't forget tooclose MessageOutput tooget logs into file
                MessageOutput.Close();
            }
        }
    }

    namespace Test.Helpers
    {
        public static class FileAssert
        {
            static string GetFileHash(string filename)
            {
                Assert.IsTrue(File.Exists(filename));

                using (var hash = new SHA1Managed())
                {
                    var clearBytes = File.ReadAllBytes(filename);
                    var hashedBytes = hash.ComputeHash(clearBytes);
                    return ConvertBytesToHex(hashedBytes);
                }
            }

            static string ConvertBytesToHex(byte[] bytes)
            {
                var sb = new StringBuilder();

                for (var i = 0; i < bytes.Length; i++)
                {
                    sb.Append(bytes[i].ToString("x"));
                }
                return sb.ToString();
            }

            public static void AreEqual(string filename1, string filename2)
            {
                string hash1 = GetFileHash(filename1);
                string hash2 = GetFileHash(filename2);

                Assert.AreEqual(hash1, hash2);
            }
        }
    }


### <a name="programming-interfaces-in-localrunhelperexe"></a><span data-ttu-id="32b75-327">Programmeringsgränssnitt i LocalRunHelper.exe</span><span class="sxs-lookup"><span data-stu-id="32b75-327">Programming interfaces in LocalRunHelper.exe</span></span>

<span data-ttu-id="32b75-328">LocalRunHelper.exe innehåller hello programmeringsgränssnitt för U-SQL lokalt kompilera kör, etc. hello gränssnitt visas nedan.</span><span class="sxs-lookup"><span data-stu-id="32b75-328">LocalRunHelper.exe provides hello programming interfaces for U-SQL local compile, run, etc. hello interfaces are listed as follows.</span></span>

<span data-ttu-id="32b75-329">**Konstruktorn**</span><span class="sxs-lookup"><span data-stu-id="32b75-329">**Constructor**</span></span>

<span data-ttu-id="32b75-330">offentliga LocalRunHelper ([System.IO.TextWriter messageOutput = null])</span><span class="sxs-lookup"><span data-stu-id="32b75-330">public LocalRunHelper([System.IO.TextWriter messageOutput = null])</span></span>

|<span data-ttu-id="32b75-331">Parameter</span><span class="sxs-lookup"><span data-stu-id="32b75-331">Parameter</span></span>|<span data-ttu-id="32b75-332">Typ</span><span class="sxs-lookup"><span data-stu-id="32b75-332">Type</span></span>|<span data-ttu-id="32b75-333">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="32b75-333">Description</span></span>|
|---------|----|-----------|
|<span data-ttu-id="32b75-334">messageOutput</span><span class="sxs-lookup"><span data-stu-id="32b75-334">messageOutput</span></span>|<span data-ttu-id="32b75-335">System.IO.TextWriter</span><span class="sxs-lookup"><span data-stu-id="32b75-335">System.IO.TextWriter</span></span>|<span data-ttu-id="32b75-336">Ange toonull toouse konsolen för utgående meddelanden.</span><span class="sxs-lookup"><span data-stu-id="32b75-336">for output messages, set toonull toouse Console</span></span>|

<span data-ttu-id="32b75-337">**Egenskaper**</span><span class="sxs-lookup"><span data-stu-id="32b75-337">**Properties**</span></span>

|<span data-ttu-id="32b75-338">Egenskap</span><span class="sxs-lookup"><span data-stu-id="32b75-338">Property</span></span>|<span data-ttu-id="32b75-339">Typ</span><span class="sxs-lookup"><span data-stu-id="32b75-339">Type</span></span>|<span data-ttu-id="32b75-340">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="32b75-340">Description</span></span>|
|--------|----|-----------|
|<span data-ttu-id="32b75-341">AlgebraPath</span><span class="sxs-lookup"><span data-stu-id="32b75-341">AlgebraPath</span></span>|<span data-ttu-id="32b75-342">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-342">string</span></span>|<span data-ttu-id="32b75-343">hello sökväg tooalgebra fil (algebra filen är en av hello Kompileringsresultatet)</span><span class="sxs-lookup"><span data-stu-id="32b75-343">hello path tooalgebra file (algebra file is one of hello compilation results)</span></span>|
|<span data-ttu-id="32b75-344">CodeBehindReferences</span><span class="sxs-lookup"><span data-stu-id="32b75-344">CodeBehindReferences</span></span>|<span data-ttu-id="32b75-345">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-345">string</span></span>|<span data-ttu-id="32b75-346">Hello skript finns ytterligare kod bakom referenser anger hello sökvägar avgränsade med ';'</span><span class="sxs-lookup"><span data-stu-id="32b75-346">If hello script has additional code behind references, specify hello paths separated with ';'</span></span>|
|<span data-ttu-id="32b75-347">CppSdkDir</span><span class="sxs-lookup"><span data-stu-id="32b75-347">CppSdkDir</span></span>|<span data-ttu-id="32b75-348">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-348">string</span></span>|<span data-ttu-id="32b75-349">CppSDK directory</span><span class="sxs-lookup"><span data-stu-id="32b75-349">CppSDK directory</span></span>|
|<span data-ttu-id="32b75-350">CurrentDir</span><span class="sxs-lookup"><span data-stu-id="32b75-350">CurrentDir</span></span>|<span data-ttu-id="32b75-351">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-351">string</span></span>|<span data-ttu-id="32b75-352">Aktuell katalog</span><span class="sxs-lookup"><span data-stu-id="32b75-352">Current directory</span></span>|
|<span data-ttu-id="32b75-353">DataRoot</span><span class="sxs-lookup"><span data-stu-id="32b75-353">DataRoot</span></span>|<span data-ttu-id="32b75-354">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-354">string</span></span>|<span data-ttu-id="32b75-355">Rotsökvägen för data</span><span class="sxs-lookup"><span data-stu-id="32b75-355">Data root path</span></span>|
|<span data-ttu-id="32b75-356">DebuggerMailPath</span><span class="sxs-lookup"><span data-stu-id="32b75-356">DebuggerMailPath</span></span>|<span data-ttu-id="32b75-357">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-357">string</span></span>|<span data-ttu-id="32b75-358">hello sökvägen toodebugger mailslot</span><span class="sxs-lookup"><span data-stu-id="32b75-358">hello path toodebugger mailslot</span></span>|
|<span data-ttu-id="32b75-359">GenerateUdoRedirect</span><span class="sxs-lookup"><span data-stu-id="32b75-359">GenerateUdoRedirect</span></span>|<span data-ttu-id="32b75-360">bool</span><span class="sxs-lookup"><span data-stu-id="32b75-360">bool</span></span>|<span data-ttu-id="32b75-361">Om vi vill toogenerate sammansättning lästes in omdirigering åsidosätta config</span><span class="sxs-lookup"><span data-stu-id="32b75-361">If we want toogenerate assembly loading redirection override config</span></span>|
|<span data-ttu-id="32b75-362">HasCodeBehind</span><span class="sxs-lookup"><span data-stu-id="32b75-362">HasCodeBehind</span></span>|<span data-ttu-id="32b75-363">bool</span><span class="sxs-lookup"><span data-stu-id="32b75-363">bool</span></span>|<span data-ttu-id="32b75-364">Om hello skript har bakomliggande kod</span><span class="sxs-lookup"><span data-stu-id="32b75-364">If hello script has code behind</span></span>|
|<span data-ttu-id="32b75-365">InputDir</span><span class="sxs-lookup"><span data-stu-id="32b75-365">InputDir</span></span>|<span data-ttu-id="32b75-366">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-366">string</span></span>|<span data-ttu-id="32b75-367">Katalogen för indata</span><span class="sxs-lookup"><span data-stu-id="32b75-367">Directory for input data</span></span>|
|<span data-ttu-id="32b75-368">MessagePath</span><span class="sxs-lookup"><span data-stu-id="32b75-368">MessagePath</span></span>|<span data-ttu-id="32b75-369">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-369">string</span></span>|<span data-ttu-id="32b75-370">Meddelandet dump sökväg</span><span class="sxs-lookup"><span data-stu-id="32b75-370">Message dump file path</span></span>|
|<span data-ttu-id="32b75-371">OutputDir</span><span class="sxs-lookup"><span data-stu-id="32b75-371">OutputDir</span></span>|<span data-ttu-id="32b75-372">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-372">string</span></span>|<span data-ttu-id="32b75-373">Katalogen för utdata</span><span class="sxs-lookup"><span data-stu-id="32b75-373">Directory for output data</span></span>|
|<span data-ttu-id="32b75-374">Parallellitet</span><span class="sxs-lookup"><span data-stu-id="32b75-374">Parallelism</span></span>|<span data-ttu-id="32b75-375">int</span><span class="sxs-lookup"><span data-stu-id="32b75-375">int</span></span>|<span data-ttu-id="32b75-376">Parallellitet toorun hello algebra</span><span class="sxs-lookup"><span data-stu-id="32b75-376">Parallelism toorun hello algebra</span></span>|
|<span data-ttu-id="32b75-377">ParentPid</span><span class="sxs-lookup"><span data-stu-id="32b75-377">ParentPid</span></span>|<span data-ttu-id="32b75-378">int</span><span class="sxs-lookup"><span data-stu-id="32b75-378">int</span></span>|<span data-ttu-id="32b75-379">Process-ID för hello överordnade på vilka hello tjänsten övervakar tooexit, ange too0 eller negativa tooignore</span><span class="sxs-lookup"><span data-stu-id="32b75-379">PID of hello parent on which hello service monitors tooexit, set too0 or negative tooignore</span></span>|
|<span data-ttu-id="32b75-380">ResultPath</span><span class="sxs-lookup"><span data-stu-id="32b75-380">ResultPath</span></span>|<span data-ttu-id="32b75-381">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-381">string</span></span>|<span data-ttu-id="32b75-382">Resultatet dump sökväg</span><span class="sxs-lookup"><span data-stu-id="32b75-382">Result dump file path</span></span>|
|<span data-ttu-id="32b75-383">RuntimeDir</span><span class="sxs-lookup"><span data-stu-id="32b75-383">RuntimeDir</span></span>|<span data-ttu-id="32b75-384">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-384">string</span></span>|<span data-ttu-id="32b75-385">Runtime directory</span><span class="sxs-lookup"><span data-stu-id="32b75-385">Runtime directory</span></span>|
|<span data-ttu-id="32b75-386">ScriptPath</span><span class="sxs-lookup"><span data-stu-id="32b75-386">ScriptPath</span></span>|<span data-ttu-id="32b75-387">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-387">string</span></span>|<span data-ttu-id="32b75-388">Där toofind hello skript</span><span class="sxs-lookup"><span data-stu-id="32b75-388">Where toofind hello script</span></span>|
|<span data-ttu-id="32b75-389">Lite</span><span class="sxs-lookup"><span data-stu-id="32b75-389">Shallow</span></span>|<span data-ttu-id="32b75-390">bool</span><span class="sxs-lookup"><span data-stu-id="32b75-390">bool</span></span>|<span data-ttu-id="32b75-391">Ytlig kompilera eller inte</span><span class="sxs-lookup"><span data-stu-id="32b75-391">Shallow compile or not</span></span>|
|<span data-ttu-id="32b75-392">TempDir</span><span class="sxs-lookup"><span data-stu-id="32b75-392">TempDir</span></span>|<span data-ttu-id="32b75-393">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-393">string</span></span>|<span data-ttu-id="32b75-394">Temp-katalogen</span><span class="sxs-lookup"><span data-stu-id="32b75-394">Temp directory</span></span>|
|<span data-ttu-id="32b75-395">UseDataBase</span><span class="sxs-lookup"><span data-stu-id="32b75-395">UseDataBase</span></span>|<span data-ttu-id="32b75-396">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-396">string</span></span>|<span data-ttu-id="32b75-397">Ange hello databasen toouse för koden bakom tillfällig sammansättning registrering, master som standard</span><span class="sxs-lookup"><span data-stu-id="32b75-397">Specify hello database toouse for code behind temporary assembly registration, master by default</span></span>|
|<span data-ttu-id="32b75-398">WorkDir</span><span class="sxs-lookup"><span data-stu-id="32b75-398">WorkDir</span></span>|<span data-ttu-id="32b75-399">Sträng</span><span class="sxs-lookup"><span data-stu-id="32b75-399">string</span></span>|<span data-ttu-id="32b75-400">Prioriterade arbetskatalogen</span><span class="sxs-lookup"><span data-stu-id="32b75-400">Preferred working directory</span></span>|


<span data-ttu-id="32b75-401">**Metoden**</span><span class="sxs-lookup"><span data-stu-id="32b75-401">**Method**</span></span>

|<span data-ttu-id="32b75-402">Metod</span><span class="sxs-lookup"><span data-stu-id="32b75-402">Method</span></span>|<span data-ttu-id="32b75-403">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="32b75-403">Description</span></span>|<span data-ttu-id="32b75-404">Returnera</span><span class="sxs-lookup"><span data-stu-id="32b75-404">Return</span></span>|<span data-ttu-id="32b75-405">Parameter</span><span class="sxs-lookup"><span data-stu-id="32b75-405">Parameter</span></span>|
|------|-----------|------|---------|
|<span data-ttu-id="32b75-406">offentliga bool DoCompile()</span><span class="sxs-lookup"><span data-stu-id="32b75-406">public bool DoCompile()</span></span>|<span data-ttu-id="32b75-407">Kompilera hello U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="32b75-407">Compile hello U-SQL script</span></span>|<span data-ttu-id="32b75-408">SANT lyckades</span><span class="sxs-lookup"><span data-stu-id="32b75-408">True on success</span></span>| |
|<span data-ttu-id="32b75-409">offentliga bool DoExec()</span><span class="sxs-lookup"><span data-stu-id="32b75-409">public bool DoExec()</span></span>|<span data-ttu-id="32b75-410">Köra hello kompileras resultat</span><span class="sxs-lookup"><span data-stu-id="32b75-410">Execute hello compiled result</span></span>|<span data-ttu-id="32b75-411">SANT lyckades</span><span class="sxs-lookup"><span data-stu-id="32b75-411">True on success</span></span>| |
|<span data-ttu-id="32b75-412">offentliga bool DoRun()</span><span class="sxs-lookup"><span data-stu-id="32b75-412">public bool DoRun()</span></span>|<span data-ttu-id="32b75-413">Kör hello U-SQL-skript (kompilera + Execute)</span><span class="sxs-lookup"><span data-stu-id="32b75-413">Run hello U-SQL script (Compile + Execute)</span></span>|<span data-ttu-id="32b75-414">SANT lyckades</span><span class="sxs-lookup"><span data-stu-id="32b75-414">True on success</span></span>| |
|<span data-ttu-id="32b75-415">offentliga bool IsValidRuntimeDir (sträng sökväg)</span><span class="sxs-lookup"><span data-stu-id="32b75-415">public bool IsValidRuntimeDir(string path)</span></span>|<span data-ttu-id="32b75-416">Kontrollera om hello angivna sökvägen är giltig runtime sökväg</span><span class="sxs-lookup"><span data-stu-id="32b75-416">Check if hello given path is valid runtime path</span></span>|<span data-ttu-id="32b75-417">Sant för giltigt</span><span class="sxs-lookup"><span data-stu-id="32b75-417">True for valid</span></span>|<span data-ttu-id="32b75-418">hello sökvägen till runtime-katalog</span><span class="sxs-lookup"><span data-stu-id="32b75-418">hello path of runtime directory</span></span>|


## <a name="faq-about-common-issue"></a><span data-ttu-id="32b75-419">Vanliga frågor och svar om vanliga problem</span><span class="sxs-lookup"><span data-stu-id="32b75-419">FAQ about common issue</span></span>

### <a name="error-1"></a><span data-ttu-id="32b75-420">Fel 1:</span><span class="sxs-lookup"><span data-stu-id="32b75-420">Error 1:</span></span>
<span data-ttu-id="32b75-421">E_CSC_SYSTEM_INTERNAL: Internt fel!</span><span class="sxs-lookup"><span data-stu-id="32b75-421">E_CSC_SYSTEM_INTERNAL: Internal error!</span></span> <span data-ttu-id="32b75-422">Det gick inte att läsa in filen eller sammansättningen 'ScopeEngineManaged.dll' eller en av dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="32b75-422">Could not load file or assembly 'ScopeEngineManaged.dll' or one of its dependencies.</span></span> <span data-ttu-id="32b75-423">Det gick inte att hitta hello angiven modul.</span><span class="sxs-lookup"><span data-stu-id="32b75-423">hello specified module could not be found.</span></span>

<span data-ttu-id="32b75-424">Kontrollera hello följande:</span><span class="sxs-lookup"><span data-stu-id="32b75-424">Please check hello following:</span></span>

- <span data-ttu-id="32b75-425">Kontrollera att du har x64 miljö.</span><span class="sxs-lookup"><span data-stu-id="32b75-425">Make sure you have x64 environment.</span></span> <span data-ttu-id="32b75-426">hello build målplattform och hello testmiljö bör vara x64 finns för**steg 1: skapa C# enhet Testa projektet och konfiguration** ovan.</span><span class="sxs-lookup"><span data-stu-id="32b75-426">hello build target platform and hello test environment should be x64, refer too**Step 1: Create C# unit test project and configuration** above.</span></span>
- <span data-ttu-id="32b75-427">Kontrollera att du har kopierat alla beroende filer under NugetPackage\build\runtime\ tooproject arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="32b75-427">Make sure you have copied all dependency files under NugetPackage\build\runtime\ tooproject working directory.</span></span>


## <a name="next-steps"></a><span data-ttu-id="32b75-428">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32b75-428">Next steps</span></span>

* <span data-ttu-id="32b75-429">toolearn U-SQL finns [Kom igång med Azure Data Lake Analytics U-SQL-språket](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="32b75-429">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="32b75-430">toolog diagnostikinformation, se [åtkomst till diagnostik loggar för Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="32b75-430">toolog diagnostics information, see [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).</span></span>
* <span data-ttu-id="32b75-431">toosee en mer komplex fråga, se [analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="32b75-431">toosee a more complex query, see [Analyze website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="32b75-432">tooview jobbinformation finns [Använd jobbet webbläsare och visa jobb för Azure Data Lake Analytics-jobb](data-lake-analytics-data-lake-tools-view-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="32b75-432">tooview job details, see [Use Job Browser and Job View for Azure Data Lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md).</span></span>
* <span data-ttu-id="32b75-433">toouse hello vertex vy, se [Använd hello Vertex vy i Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span><span class="sxs-lookup"><span data-stu-id="32b75-433">toouse hello vertex execution view, see [Use hello Vertex Execution View in Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span></span>
