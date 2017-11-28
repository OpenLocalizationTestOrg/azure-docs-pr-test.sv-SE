---
title: "Skala lokal U-SQL-köra och testa med Azure Data Lake U-SQL-SDK | Microsoft Docs"
description: "Lär dig hur du använder Azure Data Lake U-SQL-SDK för att skala U-SQL lokalt körs jobben och testning med kommandoraden och programmeringsgränssnitt på din lokala arbetsstationen."
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
ms.openlocfilehash: 55242bcf644ca0e7f30cfe7eada2130451c36e64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="scale-u-sql-local-run-and-test-with-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="292cd-103">Skala lokal U-SQL-köra och testa med Azure Data Lake U-SQL-SDK</span><span class="sxs-lookup"><span data-stu-id="292cd-103">Scale U-SQL local run and test with Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="292cd-104">När du utvecklar U-SQL-skript, är det vanligt att köra och testa U-SQL-skript lokalt skicka innan den till molnet.</span><span class="sxs-lookup"><span data-stu-id="292cd-104">When developing U-SQL script, it is common to run and test U-SQL script locally before submit it to cloud.</span></span> <span data-ttu-id="292cd-105">Azure Data Lake innehåller en Nuget-paketet kallas SDK för Azure Data Lake U-SQL i det här scenariot, via som du sedan kan du skala lokal U-SQL kör och test.</span><span class="sxs-lookup"><span data-stu-id="292cd-105">Azure Data Lake provides a Nuget package called Azure Data Lake U-SQL SDK for this scenario, through which you can easily scale U-SQL local run and test.</span></span> <span data-ttu-id="292cd-106">Det är också möjligt att integrera U-SQL testet med CI (kontinuerlig Integration) system för att automatisera kompileringen och testa.</span><span class="sxs-lookup"><span data-stu-id="292cd-106">It is also possible to integrate this U-SQL test with CI (Continuous Integration) system to automate the compile and test.</span></span>

<span data-ttu-id="292cd-107">Om du bryr dig om hur för att manuellt lokala körs och felsöka U-SQL-skript med GUI-tooling kan du använda Azure Data Lake-verktyg för Visual Studio för att.</span><span class="sxs-lookup"><span data-stu-id="292cd-107">If you care about how to manually local run and debug U-SQL script with GUI tooling, then you can use Azure Data Lake Tools for Visual Studio for that.</span></span> <span data-ttu-id="292cd-108">Mer information från [här](data-lake-analytics-data-lake-tools-local-run.md).</span><span class="sxs-lookup"><span data-stu-id="292cd-108">You can learn more from [here](data-lake-analytics-data-lake-tools-local-run.md).</span></span>

## <a name="install-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="292cd-109">Installera Azure Data Lake U-SQL-SDK</span><span class="sxs-lookup"><span data-stu-id="292cd-109">Install Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="292cd-110">Du kan hämta SDK för Azure Data Lake U-SQL [här](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) på Nuget.org.</span><span class="sxs-lookup"><span data-stu-id="292cd-110">You can get the Azure Data Lake U-SQL SDK [here](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) on Nuget.org.</span></span> <span data-ttu-id="292cd-111">Och innan du använder den, måste du kontrollera att du har beroenden på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="292cd-111">And before using it, you need to make sure you have dependencies as follows.</span></span>

### <a name="dependencies"></a><span data-ttu-id="292cd-112">Beroenden</span><span class="sxs-lookup"><span data-stu-id="292cd-112">Dependencies</span></span>

<span data-ttu-id="292cd-113">Data Lake U-SQL-SDK kräver följande beroenden:</span><span class="sxs-lookup"><span data-stu-id="292cd-113">The Data Lake U-SQL SDK requires the following dependencies:</span></span>

- <span data-ttu-id="292cd-114">[Microsoft .NET Framework 4.6 eller nyare](https://www.microsoft.com/download/details.aspx?id=17851).</span><span class="sxs-lookup"><span data-stu-id="292cd-114">[Microsoft .NET Framework 4.6 or newer](https://www.microsoft.com/download/details.aspx?id=17851).</span></span>
- <span data-ttu-id="292cd-115">Microsoft Visual C++-14 och Windows SDK 10.0.10240.0 eller senare (som kallas CppSDK i den här artikeln).</span><span class="sxs-lookup"><span data-stu-id="292cd-115">Microsoft Visual C++ 14 and Windows SDK 10.0.10240.0 or newer (which is called CppSDK in this article).</span></span> <span data-ttu-id="292cd-116">Det finns två sätt att få CppSDK:</span><span class="sxs-lookup"><span data-stu-id="292cd-116">There are two ways to get CppSDK:</span></span>

    - <span data-ttu-id="292cd-117">Installera [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou).</span><span class="sxs-lookup"><span data-stu-id="292cd-117">Install [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou).</span></span> <span data-ttu-id="292cd-118">Du har en \Windows Kits\10 mappen under mappen Program – till exempel C:\Program Files (x86) \Windows Kits\10\.</span><span class="sxs-lookup"><span data-stu-id="292cd-118">You'll have a \Windows Kits\10 folder under the Program Files folder--for example, C:\Program Files (x86)\Windows Kits\10\.</span></span> <span data-ttu-id="292cd-119">Du hittar också SDK för Windows 10-version i \Windows Kits\10\Lib.</span><span class="sxs-lookup"><span data-stu-id="292cd-119">You'll also find the Windows 10 SDK version under \Windows Kits\10\Lib.</span></span> <span data-ttu-id="292cd-120">Om du inte ser dessa mappar, installera om Visual Studio och se till att välja Windows 10 SDK under installationen.</span><span class="sxs-lookup"><span data-stu-id="292cd-120">If you don’t see these folders, reinstall Visual Studio and be sure to select the Windows 10 SDK during the installation.</span></span> <span data-ttu-id="292cd-121">Om du har det installeras med Visual Studio, hittar lokal U-SQL-kompileraren den automatiskt.</span><span class="sxs-lookup"><span data-stu-id="292cd-121">If you have this installed with Visual Studio, the U-SQL local compiler will find it automatically.</span></span>

    ![Data Lake-verktyg för Visual Studio lokala kör Windows 10-SDK](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

    - <span data-ttu-id="292cd-123">Installera [Data Lake-verktyg för Visual Studio](http://aka.ms/adltoolsvs).</span><span class="sxs-lookup"><span data-stu-id="292cd-123">Install [Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs).</span></span> <span data-ttu-id="292cd-124">Du kan hitta färdigförpackade Visual C++ och Windows SDK filer i C:\Program Files (x86) \Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK.</span><span class="sxs-lookup"><span data-stu-id="292cd-124">You can find the prepackaged Visual C++ and Windows SDK files at C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK.</span></span> <span data-ttu-id="292cd-125">I det här fallet hittar lokal U-SQL-kompileraren inte beroenden automatiskt.</span><span class="sxs-lookup"><span data-stu-id="292cd-125">In this case, the U-SQL local compiler cannot find the dependencies automatically.</span></span> <span data-ttu-id="292cd-126">Du måste ange CppSDK sökvägen för den.</span><span class="sxs-lookup"><span data-stu-id="292cd-126">You need to specify the CppSDK path for it.</span></span> <span data-ttu-id="292cd-127">Du kan kopiera filer till en annan plats, eller så kan du använda den som den är.</span><span class="sxs-lookup"><span data-stu-id="292cd-127">You can either copy the files to another location or use it as is.</span></span>

## <a name="understand-basic-concepts"></a><span data-ttu-id="292cd-128">Förstå grundläggande begrepp</span><span class="sxs-lookup"><span data-stu-id="292cd-128">Understand basic concepts</span></span>

### <a name="data-root"></a><span data-ttu-id="292cd-129">Dataroten</span><span class="sxs-lookup"><span data-stu-id="292cd-129">Data root</span></span>

<span data-ttu-id="292cd-130">Data-rotmappen är ”lokalt Arkiv” för den lokala beräkningskonto.</span><span class="sxs-lookup"><span data-stu-id="292cd-130">The data-root folder is a "local store" for the local compute account.</span></span> <span data-ttu-id="292cd-131">Det motsvarar att Azure Data Lake Store-konto för Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="292cd-131">It's equivalent to the Azure Data Lake Store account of a Data Lake Analytics account.</span></span> <span data-ttu-id="292cd-132">Växla till en annan data-rotmapp är precis som om du växlar till en annan store-konto.</span><span class="sxs-lookup"><span data-stu-id="292cd-132">Switching to a different data-root folder is just like switching to a different store account.</span></span> <span data-ttu-id="292cd-133">Om du vill komma åt ofta delade data med olika dataroten mappar måste du använda absoluta sökvägar i skript.</span><span class="sxs-lookup"><span data-stu-id="292cd-133">If you want to access commonly shared data with different data-root folders, you must use absolute paths in your scripts.</span></span> <span data-ttu-id="292cd-134">Skapa filen system symboliska länkar (till exempel **mklink** på NTFS) under data till rotmappen att peka till delade data.</span><span class="sxs-lookup"><span data-stu-id="292cd-134">Or, create file system symbolic links (for example, **mklink** on NTFS) under the data-root folder to point to the shared data.</span></span>

<span data-ttu-id="292cd-135">Data-rotmappen används för att:</span><span class="sxs-lookup"><span data-stu-id="292cd-135">The data-root folder is used to:</span></span>

- <span data-ttu-id="292cd-136">Lagra lokala metadata, inklusive databaser, tabeller, tabellvärdesfunktioner (Tabellvärdesfunktioner) och sammansättningar.</span><span class="sxs-lookup"><span data-stu-id="292cd-136">Store local metadata, including databases, tables, table-valued functions (TVFs), and assemblies.</span></span>
- <span data-ttu-id="292cd-137">Leta upp de inkommande och utgående sökvägar som har definierats som relativa sökvägar i U-SQL.</span><span class="sxs-lookup"><span data-stu-id="292cd-137">Look up the input and output paths that are defined as relative paths in U-SQL.</span></span> <span data-ttu-id="292cd-138">Använda relativa sökvägar gör det enklare att distribuera dina U-SQL-projekt till Azure.</span><span class="sxs-lookup"><span data-stu-id="292cd-138">Using relative paths makes it easier to deploy your U-SQL projects to Azure.</span></span>

### <a name="file-path-in-u-sql"></a><span data-ttu-id="292cd-139">Sökvägen i U-SQL</span><span class="sxs-lookup"><span data-stu-id="292cd-139">File path in U-SQL</span></span>

<span data-ttu-id="292cd-140">Du kan använda både en relativ sökväg och en lokal absolut sökväg i U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="292cd-140">You can use both a relative path and a local absolute path in U-SQL scripts.</span></span> <span data-ttu-id="292cd-141">Den relativa sökvägen är i förhållande till angivna data-sökvägen till rotmappen.</span><span class="sxs-lookup"><span data-stu-id="292cd-141">The relative path is relative to the specified data-root folder path.</span></span> <span data-ttu-id="292cd-142">Vi rekommenderar att du använder ”/” som sökväg avgränsare att göra skript som är kompatibel med servern.</span><span class="sxs-lookup"><span data-stu-id="292cd-142">We recommend that you use "/" as the path separator to make your scripts compatible with the server side.</span></span> <span data-ttu-id="292cd-143">Här följer några exempel på relativa sökvägar och deras motsvarande absoluta sökvägar.</span><span class="sxs-lookup"><span data-stu-id="292cd-143">Here are some examples of relative paths and their equivalent absolute paths.</span></span> <span data-ttu-id="292cd-144">I det här exemplet är C:\LocalRunDataRoot data till rotmappen.</span><span class="sxs-lookup"><span data-stu-id="292cd-144">In these examples, C:\LocalRunDataRoot is the data-root folder.</span></span>

|<span data-ttu-id="292cd-145">Relativ sökväg</span><span class="sxs-lookup"><span data-stu-id="292cd-145">Relative path</span></span>|<span data-ttu-id="292cd-146">Absolut sökväg</span><span class="sxs-lookup"><span data-stu-id="292cd-146">Absolute path</span></span>|
|-------------|-------------|
|<span data-ttu-id="292cd-147">/ABC/def/Input.csv</span><span class="sxs-lookup"><span data-stu-id="292cd-147">/abc/def/input.csv</span></span> |<span data-ttu-id="292cd-148">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="292cd-148">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="292cd-149">ABC/def/Input.csv</span><span class="sxs-lookup"><span data-stu-id="292cd-149">abc/def/input.csv</span></span>  |<span data-ttu-id="292cd-150">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="292cd-150">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="292cd-151">D:/ABC/def/Input.csv</span><span class="sxs-lookup"><span data-stu-id="292cd-151">D:/abc/def/input.csv</span></span> |<span data-ttu-id="292cd-152">D:\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="292cd-152">D:\abc\def\input.csv</span></span>|

### <a name="working-directory"></a><span data-ttu-id="292cd-153">Arbetskatalog</span><span class="sxs-lookup"><span data-stu-id="292cd-153">Working directory</span></span>

<span data-ttu-id="292cd-154">När du kör U-SQL-skript lokalt, skapas en arbetskatalog under kompilering under katalogen körs.</span><span class="sxs-lookup"><span data-stu-id="292cd-154">When running the U-SQL script locally, a working directory is created during compilation under current running directory.</span></span> <span data-ttu-id="292cd-155">Förutom kompilering-utdata blir nödvändiga runtime-filer för lokala körningen shadow kopieras till den här arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="292cd-155">In addition to the compilation outputs, the needed runtime files for local execution will be shadow copied to this working directory.</span></span> <span data-ttu-id="292cd-156">Rotmapp för aktiv katalog kallas ”ScopeWorkDir” och filer under arbetskatalogen är följande:</span><span class="sxs-lookup"><span data-stu-id="292cd-156">The working directory root folder is called "ScopeWorkDir" and the files under the working directory are as follows:</span></span>

|<span data-ttu-id="292cd-157">Filen/katalogen</span><span class="sxs-lookup"><span data-stu-id="292cd-157">Directory/file</span></span>|<span data-ttu-id="292cd-158">Filen/katalogen</span><span class="sxs-lookup"><span data-stu-id="292cd-158">Directory/file</span></span>|<span data-ttu-id="292cd-159">Filen/katalogen</span><span class="sxs-lookup"><span data-stu-id="292cd-159">Directory/file</span></span>|<span data-ttu-id="292cd-160">Definition</span><span class="sxs-lookup"><span data-stu-id="292cd-160">Definition</span></span>|<span data-ttu-id="292cd-161">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="292cd-161">Description</span></span>|
|--------------|--------------|--------------|----------|-----------|
|<span data-ttu-id="292cd-162">C6A101DDCB470506</span><span class="sxs-lookup"><span data-stu-id="292cd-162">C6A101DDCB470506</span></span>| | |<span data-ttu-id="292cd-163">Hash-sträng av körningsversion</span><span class="sxs-lookup"><span data-stu-id="292cd-163">Hash string of runtime version</span></span>|<span data-ttu-id="292cd-164">Skuggkopia av runtime-filer som behövs för lokal körning</span><span class="sxs-lookup"><span data-stu-id="292cd-164">Shadow copy of runtime files needed for local execution</span></span>|
| |<span data-ttu-id="292cd-165">Script_66AE4909AA0ED06C</span><span class="sxs-lookup"><span data-stu-id="292cd-165">Script_66AE4909AA0ED06C</span></span>| |<span data-ttu-id="292cd-166">Script namn + hash-sträng med sökvägen för skriptet</span><span class="sxs-lookup"><span data-stu-id="292cd-166">Script name + hash string of script path</span></span>|<span data-ttu-id="292cd-167">Utdata för kompilering och körning steg loggning</span><span class="sxs-lookup"><span data-stu-id="292cd-167">Compilation outputs and execution step logging</span></span>|
| | |<span data-ttu-id="292cd-168">\_skriptet\_.abr</span><span class="sxs-lookup"><span data-stu-id="292cd-168">\_script\_.abr</span></span>|<span data-ttu-id="292cd-169">Utdata för kompilator</span><span class="sxs-lookup"><span data-stu-id="292cd-169">Compiler output</span></span>|<span data-ttu-id="292cd-170">Algebra fil</span><span class="sxs-lookup"><span data-stu-id="292cd-170">Algebra file</span></span>|
| | |<span data-ttu-id="292cd-171">\_ScopeCodeGen\_. *</span><span class="sxs-lookup"><span data-stu-id="292cd-171">\_ScopeCodeGen\_.*</span></span>|<span data-ttu-id="292cd-172">Utdata för kompilator</span><span class="sxs-lookup"><span data-stu-id="292cd-172">Compiler output</span></span>|<span data-ttu-id="292cd-173">Genererade hanterad kod</span><span class="sxs-lookup"><span data-stu-id="292cd-173">Generated managed code</span></span>|
| | |<span data-ttu-id="292cd-174">\_ScopeCodeGenEngine\_. *</span><span class="sxs-lookup"><span data-stu-id="292cd-174">\_ScopeCodeGenEngine\_.*</span></span>|<span data-ttu-id="292cd-175">Utdata för kompilator</span><span class="sxs-lookup"><span data-stu-id="292cd-175">Compiler output</span></span>|<span data-ttu-id="292cd-176">Genererade maskinkod</span><span class="sxs-lookup"><span data-stu-id="292cd-176">Generated native code</span></span>|
| | |<span data-ttu-id="292cd-177">Refererade sammansättningar</span><span class="sxs-lookup"><span data-stu-id="292cd-177">referenced assemblies</span></span>|<span data-ttu-id="292cd-178">Sammansättningsreferensen</span><span class="sxs-lookup"><span data-stu-id="292cd-178">Assembly reference</span></span>|<span data-ttu-id="292cd-179">Sammansättningsfiler</span><span class="sxs-lookup"><span data-stu-id="292cd-179">Referenced assembly files</span></span>|
| | |<span data-ttu-id="292cd-180">deployed_resources</span><span class="sxs-lookup"><span data-stu-id="292cd-180">deployed_resources</span></span>|<span data-ttu-id="292cd-181">Resurs-distribution</span><span class="sxs-lookup"><span data-stu-id="292cd-181">Resource deployment</span></span>|<span data-ttu-id="292cd-182">Resursfiler för distribution</span><span class="sxs-lookup"><span data-stu-id="292cd-182">Resource deployment files</span></span>|
| | |<span data-ttu-id="292cd-183">xxxxxxxx.xxx[1..n]\_\*. *</span><span class="sxs-lookup"><span data-stu-id="292cd-183">xxxxxxxx.xxx[1..n]\_\*.*</span></span>|<span data-ttu-id="292cd-184">Körningsloggen</span><span class="sxs-lookup"><span data-stu-id="292cd-184">Execution log</span></span>|<span data-ttu-id="292cd-185">Logg över utförande</span><span class="sxs-lookup"><span data-stu-id="292cd-185">Log of execution steps</span></span>|


## <a name="use-the-sdk-from-the-command-line"></a><span data-ttu-id="292cd-186">Använd SDK: N från kommandoraden</span><span class="sxs-lookup"><span data-stu-id="292cd-186">Use the SDK from the command line</span></span>

### <a name="command-line-interface-of-the-helper-application"></a><span data-ttu-id="292cd-187">Kommandoradsgränssnitt för hjälpprogrammet</span><span class="sxs-lookup"><span data-stu-id="292cd-187">Command-line interface of the helper application</span></span>

<span data-ttu-id="292cd-188">Under SDK directory\build\runtime är LocalRunHelper.exe kommandoradsverktyget hjälpprogram som tillhandahåller gränssnitt till de flesta av funktionerna används ofta kör lokalt.</span><span class="sxs-lookup"><span data-stu-id="292cd-188">Under SDK directory\build\runtime, LocalRunHelper.exe is the command-line helper application that provides interfaces to most of the commonly used local-run functions.</span></span> <span data-ttu-id="292cd-189">Observera att både kommandot och växlarna argumentet är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="292cd-189">Note that both the command and the argument switches are case-sensitive.</span></span> <span data-ttu-id="292cd-190">Att anropa den:</span><span class="sxs-lookup"><span data-stu-id="292cd-190">To invoke it:</span></span>

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

<span data-ttu-id="292cd-191">Kör LocalRunHelper.exe utan argument eller med den **hjälp** växla till Visa hjälp:</span><span class="sxs-lookup"><span data-stu-id="292cd-191">Run LocalRunHelper.exe without arguments or with the **help** switch to show the help information:</span></span>

    > LocalRunHelper.exe help

        Command 'help' :  Show usage information
        Command 'compile' :  Compile the script
        Required Arguments :
            -Script param
                    Script File Path
        Optional Arguments :
            -Shallow [default value 'False']
                    Shallow compile

<span data-ttu-id="292cd-192">I hjälpinformation:</span><span class="sxs-lookup"><span data-stu-id="292cd-192">In the help information:</span></span>

-  <span data-ttu-id="292cd-193">**Kommandot** ger kommandots namn.</span><span class="sxs-lookup"><span data-stu-id="292cd-193">**Command** gives the command’s name.</span></span>  
-  <span data-ttu-id="292cd-194">**Argumentet är obligatoriskt** listas argument måste anges.</span><span class="sxs-lookup"><span data-stu-id="292cd-194">**Required Argument** lists arguments that must be supplied.</span></span>  
-  <span data-ttu-id="292cd-195">**Valfria argumentet** listas argument som är valfria med standardvärden.</span><span class="sxs-lookup"><span data-stu-id="292cd-195">**Optional Argument** lists arguments that are optional, with default values.</span></span>  <span data-ttu-id="292cd-196">Valfri boolesk argument ha inte parametrar, och deras utseende innebär negativt till standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="292cd-196">Optional Boolean arguments don’t have parameters, and their appearances mean negative to their default value.</span></span>

### <a name="return-value-and-logging"></a><span data-ttu-id="292cd-197">Returvärdet och loggning</span><span class="sxs-lookup"><span data-stu-id="292cd-197">Return value and logging</span></span>

<span data-ttu-id="292cd-198">Hjälpprogrammet returnerar **0** för lyckad och **-1** för felet.</span><span class="sxs-lookup"><span data-stu-id="292cd-198">The helper application returns **0** for success and **-1** for failure.</span></span> <span data-ttu-id="292cd-199">Som standard skickar alla meddelanden i hjälpen till den aktuella konsolen.</span><span class="sxs-lookup"><span data-stu-id="292cd-199">By default, the helper sends all messages to the current console.</span></span> <span data-ttu-id="292cd-200">De flesta av kommandona stöder dock den **- MessageOut sökväg_till_loggfil** valfritt argument som omdirigerar utdata till en loggfil.</span><span class="sxs-lookup"><span data-stu-id="292cd-200">However, most of the commands support the **-MessageOut path_to_log_file** optional argument that redirects the outputs to a log file.</span></span>

### <a name="environment-variable-configuring"></a><span data-ttu-id="292cd-201">Miljö variabeln konfigurera</span><span class="sxs-lookup"><span data-stu-id="292cd-201">Environment variable configuring</span></span>

<span data-ttu-id="292cd-202">Lokal U-SQL kör behov angivna data från en rotcertifikatutfärdare som konto för lokal lagring, samt en angiven CppSDK sökväg för beroenden.</span><span class="sxs-lookup"><span data-stu-id="292cd-202">U-SQL local run needs a specified data root as local storage account, as well as a specified CppSDK path for dependencies.</span></span> <span data-ttu-id="292cd-203">Du kan både ange argumentet i kommandorads- eller ange miljövariabeln för dessa.</span><span class="sxs-lookup"><span data-stu-id="292cd-203">You can both set the argument in command-line or set environment variable for them.</span></span>

- <span data-ttu-id="292cd-204">Ange den **SCOPE_CPP_SDK** miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="292cd-204">Set the **SCOPE_CPP_SDK** environment variable.</span></span>

    <span data-ttu-id="292cd-205">Om du får Microsoft Visual C++ och Windows SDK genom att installera Data Lake-verktyg för Visual Studio kan du kontrollera att du har följande mapp:</span><span class="sxs-lookup"><span data-stu-id="292cd-205">If you get Microsoft Visual C++ and the Windows SDK by installing Data Lake Tools for Visual Studio, verify that you have the following folder:</span></span>

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    <span data-ttu-id="292cd-206">Definiera en ny miljövariabel som heter **SCOPE_CPP_SDK** så att den pekar till den här katalogen.</span><span class="sxs-lookup"><span data-stu-id="292cd-206">Define a new environment variable called **SCOPE_CPP_SDK** to point to this directory.</span></span> <span data-ttu-id="292cd-207">Eller kopiera mappen till den andra platsen och ange **SCOPE_CPP_SDK** som.</span><span class="sxs-lookup"><span data-stu-id="292cd-207">Or copy the folder to the other location and specify **SCOPE_CPP_SDK** as that.</span></span>

    <span data-ttu-id="292cd-208">Förutom att konfigurera denna variabel kan du ange den **- CppSDK** argumentet när du använder kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="292cd-208">In addition to setting the environment variable, you can specify the **-CppSDK** argument when you're using the command line.</span></span> <span data-ttu-id="292cd-209">Det här argumentet skriver över dina standard CppSDK miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="292cd-209">This argument overwrites your default CppSDK environment variable.</span></span>

- <span data-ttu-id="292cd-210">Ange den **LOCALRUN_DATAROOT** miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="292cd-210">Set the **LOCALRUN_DATAROOT** environment variable.</span></span>

    <span data-ttu-id="292cd-211">Definiera en ny miljövariabel som heter **LOCALRUN_DATAROOT** som pekar på dataroten.</span><span class="sxs-lookup"><span data-stu-id="292cd-211">Define a new environment variable called **LOCALRUN_DATAROOT** that points to the data root.</span></span>

    <span data-ttu-id="292cd-212">Förutom att konfigurera denna variabel kan du ange den **- DataRoot** argumentet med data-rotsökvägen när du använder en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="292cd-212">In addition to setting the environment variable, you can specify the **-DataRoot** argument with the data-root path when you're using a command line.</span></span> <span data-ttu-id="292cd-213">Det här argumentet skriver över dina standard dataroten miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="292cd-213">This argument overwrites your default data-root environment variable.</span></span> <span data-ttu-id="292cd-214">Du måste lägga till det här argumentet var kommandoraden som du kör så att du kan skriva över miljövariabeln standard data roten för alla åtgärder.</span><span class="sxs-lookup"><span data-stu-id="292cd-214">You need to add this argument to every command line you're running so that you can overwrite the default data-root environment variable for all operations.</span></span>

### <a name="sdk-command-line-usage-samples"></a><span data-ttu-id="292cd-215">SDK kommandoraden användning prover</span><span class="sxs-lookup"><span data-stu-id="292cd-215">SDK command line usage samples</span></span>

#### <a name="compile-and-run"></a><span data-ttu-id="292cd-216">Kompilera och kör</span><span class="sxs-lookup"><span data-stu-id="292cd-216">Compile and run</span></span>

<span data-ttu-id="292cd-217">Den **kör** kommandot används för att kompilera skriptet och sedan köra kompilerade resultatet.</span><span class="sxs-lookup"><span data-stu-id="292cd-217">The **run** command is used to compile the script and then execute compiled results.</span></span> <span data-ttu-id="292cd-218">Dess kommandoradsargument är en kombination av dessa från **Kompilera** och **köra**.</span><span class="sxs-lookup"><span data-stu-id="292cd-218">Its command-line arguments are a combination of those from **compile** and **execute**.</span></span>

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

<span data-ttu-id="292cd-219">Följande är valfria argument för **kör**:</span><span class="sxs-lookup"><span data-stu-id="292cd-219">The following are optional arguments for **run**:</span></span>


|<span data-ttu-id="292cd-220">Argumentet</span><span class="sxs-lookup"><span data-stu-id="292cd-220">Argument</span></span>|<span data-ttu-id="292cd-221">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="292cd-221">Default value</span></span>|<span data-ttu-id="292cd-222">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="292cd-222">Description</span></span>|
|--------|-------------|-----------|
|<span data-ttu-id="292cd-223">-Bakomliggande koden</span><span class="sxs-lookup"><span data-stu-id="292cd-223">-CodeBehind</span></span>|<span data-ttu-id="292cd-224">False</span><span class="sxs-lookup"><span data-stu-id="292cd-224">False</span></span>|<span data-ttu-id="292cd-225">Skriptet har .cs koden bakom</span><span class="sxs-lookup"><span data-stu-id="292cd-225">The script has .cs code behind</span></span>|
|<span data-ttu-id="292cd-226">-CppSDK</span><span class="sxs-lookup"><span data-stu-id="292cd-226">-CppSDK</span></span>| |<span data-ttu-id="292cd-227">CppSDK Directory</span><span class="sxs-lookup"><span data-stu-id="292cd-227">CppSDK Directory</span></span>|
|<span data-ttu-id="292cd-228">-DataRoot</span><span class="sxs-lookup"><span data-stu-id="292cd-228">-DataRoot</span></span>| <span data-ttu-id="292cd-229">DataRoot miljövariabel</span><span class="sxs-lookup"><span data-stu-id="292cd-229">DataRoot environment variable</span></span>|<span data-ttu-id="292cd-230">DataRoot för lokal körning standard miljövariabeln 'LOCALRUN_DATAROOT'</span><span class="sxs-lookup"><span data-stu-id="292cd-230">DataRoot for local run, default to 'LOCALRUN_DATAROOT' environment variable</span></span>|
|<span data-ttu-id="292cd-231">-MessageOut</span><span class="sxs-lookup"><span data-stu-id="292cd-231">-MessageOut</span></span>| |<span data-ttu-id="292cd-232">Dump meddelanden på konsolen till en fil</span><span class="sxs-lookup"><span data-stu-id="292cd-232">Dump messages on console to a file</span></span>|
|<span data-ttu-id="292cd-233">-Parallell</span><span class="sxs-lookup"><span data-stu-id="292cd-233">-Parallel</span></span>|<span data-ttu-id="292cd-234">1</span><span class="sxs-lookup"><span data-stu-id="292cd-234">1</span></span>|<span data-ttu-id="292cd-235">Kör planen med angivna parallellitet</span><span class="sxs-lookup"><span data-stu-id="292cd-235">Run the plan with the specified parallelism</span></span>|
|<span data-ttu-id="292cd-236">-Referenser</span><span class="sxs-lookup"><span data-stu-id="292cd-236">-References</span></span>| |<span data-ttu-id="292cd-237">Lista över sökvägar till extra referenssammansättningar eller datafiler i koden bakom, avgränsade med ';'</span><span class="sxs-lookup"><span data-stu-id="292cd-237">List of paths to extra reference assemblies or data files of code behind, separated by ';'</span></span>|
|<span data-ttu-id="292cd-238">-UdoRedirect</span><span class="sxs-lookup"><span data-stu-id="292cd-238">-UdoRedirect</span></span>|<span data-ttu-id="292cd-239">False</span><span class="sxs-lookup"><span data-stu-id="292cd-239">False</span></span>|<span data-ttu-id="292cd-240">Generera Udo sammansättningen omdirigering config</span><span class="sxs-lookup"><span data-stu-id="292cd-240">Generate Udo assembly redirect config</span></span>|
|<span data-ttu-id="292cd-241">-UseDatabase</span><span class="sxs-lookup"><span data-stu-id="292cd-241">-UseDatabase</span></span>|<span data-ttu-id="292cd-242">master</span><span class="sxs-lookup"><span data-stu-id="292cd-242">master</span></span>|<span data-ttu-id="292cd-243">Databasen som ska användas för koden bakom tillfällig sammansättning registrering</span><span class="sxs-lookup"><span data-stu-id="292cd-243">Database to use for code behind temporary assembly registration</span></span>|
|<span data-ttu-id="292cd-244">-Verbose</span><span class="sxs-lookup"><span data-stu-id="292cd-244">-Verbose</span></span>|<span data-ttu-id="292cd-245">False</span><span class="sxs-lookup"><span data-stu-id="292cd-245">False</span></span>|<span data-ttu-id="292cd-246">Visa detaljerade utdata från runtime</span><span class="sxs-lookup"><span data-stu-id="292cd-246">Show detailed outputs from runtime</span></span>|
|<span data-ttu-id="292cd-247">-WorkDir</span><span class="sxs-lookup"><span data-stu-id="292cd-247">-WorkDir</span></span>|<span data-ttu-id="292cd-248">Aktuell katalog</span><span class="sxs-lookup"><span data-stu-id="292cd-248">Current Directory</span></span>|<span data-ttu-id="292cd-249">Katalogen för kompileraren användnings- och utdata</span><span class="sxs-lookup"><span data-stu-id="292cd-249">Directory for compiler usage and outputs</span></span>|
|<span data-ttu-id="292cd-250">-RunScopeCEP</span><span class="sxs-lookup"><span data-stu-id="292cd-250">-RunScopeCEP</span></span>|<span data-ttu-id="292cd-251">0</span><span class="sxs-lookup"><span data-stu-id="292cd-251">0</span></span>|<span data-ttu-id="292cd-252">ScopeCEP läge ska använda</span><span class="sxs-lookup"><span data-stu-id="292cd-252">ScopeCEP mode to use</span></span>|
|<span data-ttu-id="292cd-253">-ScopeCEPTempPath</span><span class="sxs-lookup"><span data-stu-id="292cd-253">-ScopeCEPTempPath</span></span>|<span data-ttu-id="292cd-254">Temp</span><span class="sxs-lookup"><span data-stu-id="292cd-254">temp</span></span>|<span data-ttu-id="292cd-255">Temporär sökväg för strömmande data</span><span class="sxs-lookup"><span data-stu-id="292cd-255">Temp path to use for streaming data</span></span>|
|<span data-ttu-id="292cd-256">-OptFlags</span><span class="sxs-lookup"><span data-stu-id="292cd-256">-OptFlags</span></span>| |<span data-ttu-id="292cd-257">Kommaavgränsad lista över optimering flaggor</span><span class="sxs-lookup"><span data-stu-id="292cd-257">Comma-separated list of optimizer flags</span></span>|


<span data-ttu-id="292cd-258">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="292cd-258">Here's an example:</span></span>

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

<span data-ttu-id="292cd-259">Förutom att kombinera **Kompilera** och **köra**, kan du kompilera och köra kompilerade körbara filer separat.</span><span class="sxs-lookup"><span data-stu-id="292cd-259">Besides combining **compile** and **execute**, you can compile and execute the compiled executables separately.</span></span>

#### <a name="compile-a-u-sql-script"></a><span data-ttu-id="292cd-260">Kompilera ett U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="292cd-260">Compile a U-SQL script</span></span>

<span data-ttu-id="292cd-261">Den **Kompilera** kommandot används för att kompilera ett U-SQL-skript för att körbara filer.</span><span class="sxs-lookup"><span data-stu-id="292cd-261">The **compile** command is used to compile a U-SQL script to executables.</span></span>

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

<span data-ttu-id="292cd-262">Följande är valfria argument för **Kompilera**:</span><span class="sxs-lookup"><span data-stu-id="292cd-262">The following are optional arguments for **compile**:</span></span>


|<span data-ttu-id="292cd-263">Argumentet</span><span class="sxs-lookup"><span data-stu-id="292cd-263">Argument</span></span>|<span data-ttu-id="292cd-264">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="292cd-264">Description</span></span>|
|--------|-----------|
| <span data-ttu-id="292cd-265">-Bakomliggande koden [standardvärdet 'False']</span><span class="sxs-lookup"><span data-stu-id="292cd-265">-CodeBehind [default value 'False']</span></span>|<span data-ttu-id="292cd-266">Skriptet har .cs koden bakom</span><span class="sxs-lookup"><span data-stu-id="292cd-266">The script has .cs code behind</span></span>|
| <span data-ttu-id="292cd-267">-CppSDK [standardvärde '']</span><span class="sxs-lookup"><span data-stu-id="292cd-267">-CppSDK [default value '']</span></span>|<span data-ttu-id="292cd-268">CppSDK Directory</span><span class="sxs-lookup"><span data-stu-id="292cd-268">CppSDK Directory</span></span>|
| <span data-ttu-id="292cd-269">-DataRoot [standardvärdet 'DataRoot miljövariabeln']</span><span class="sxs-lookup"><span data-stu-id="292cd-269">-DataRoot [default value 'DataRoot environment variable']</span></span>|<span data-ttu-id="292cd-270">DataRoot för lokal körning standard miljövariabeln 'LOCALRUN_DATAROOT'</span><span class="sxs-lookup"><span data-stu-id="292cd-270">DataRoot for local run, default to 'LOCALRUN_DATAROOT' environment variable</span></span>|
| <span data-ttu-id="292cd-271">-MessageOut [standardvärde '']</span><span class="sxs-lookup"><span data-stu-id="292cd-271">-MessageOut [default value '']</span></span>|<span data-ttu-id="292cd-272">Dump meddelanden på konsolen till en fil</span><span class="sxs-lookup"><span data-stu-id="292cd-272">Dump messages on console to a file</span></span>|
| <span data-ttu-id="292cd-273">-Hänvisar till [standardvärde '']</span><span class="sxs-lookup"><span data-stu-id="292cd-273">-References [default value '']</span></span>|<span data-ttu-id="292cd-274">Lista över sökvägar till extra referenssammansättningar eller datafiler i koden bakom, avgränsade med ';'</span><span class="sxs-lookup"><span data-stu-id="292cd-274">List of paths to extra reference assemblies or data files of code behind, separated by ';'</span></span>|
| <span data-ttu-id="292cd-275">-Kort [standardvärdet 'False']</span><span class="sxs-lookup"><span data-stu-id="292cd-275">-Shallow [default value 'False']</span></span>|<span data-ttu-id="292cd-276">Lite kompilera</span><span class="sxs-lookup"><span data-stu-id="292cd-276">Shallow compile</span></span>|
| <span data-ttu-id="292cd-277">-UdoRedirect [standardvärdet 'False']</span><span class="sxs-lookup"><span data-stu-id="292cd-277">-UdoRedirect [default value 'False']</span></span>|<span data-ttu-id="292cd-278">Generera Udo sammansättningen omdirigering config</span><span class="sxs-lookup"><span data-stu-id="292cd-278">Generate Udo assembly redirect config</span></span>|
| <span data-ttu-id="292cd-279">-UseDatabase [standardvärdet 'master']</span><span class="sxs-lookup"><span data-stu-id="292cd-279">-UseDatabase [default value 'master']</span></span>|<span data-ttu-id="292cd-280">Databasen som ska användas för koden bakom tillfällig sammansättning registrering</span><span class="sxs-lookup"><span data-stu-id="292cd-280">Database to use for code behind temporary assembly registration</span></span>|
| <span data-ttu-id="292cd-281">-WorkDir [standardvärdet 'Katalogen']</span><span class="sxs-lookup"><span data-stu-id="292cd-281">-WorkDir [default value 'Current Directory']</span></span>|<span data-ttu-id="292cd-282">Katalogen för kompileraren användnings- och utdata</span><span class="sxs-lookup"><span data-stu-id="292cd-282">Directory for compiler usage and outputs</span></span>|
| <span data-ttu-id="292cd-283">-RunScopeCEP [standardvärdet '0']</span><span class="sxs-lookup"><span data-stu-id="292cd-283">-RunScopeCEP [default value '0']</span></span>|<span data-ttu-id="292cd-284">ScopeCEP läge ska använda</span><span class="sxs-lookup"><span data-stu-id="292cd-284">ScopeCEP mode to use</span></span>|
| <span data-ttu-id="292cd-285">-ScopeCEPTempPath [standardvärdet 'temp']</span><span class="sxs-lookup"><span data-stu-id="292cd-285">-ScopeCEPTempPath [default value 'temp']</span></span>|<span data-ttu-id="292cd-286">Temporär sökväg för strömmande data</span><span class="sxs-lookup"><span data-stu-id="292cd-286">Temp path to use for streaming data</span></span>|
| <span data-ttu-id="292cd-287">-OptFlags [standardvärde '']</span><span class="sxs-lookup"><span data-stu-id="292cd-287">-OptFlags [default value '']</span></span>|<span data-ttu-id="292cd-288">Kommaavgränsad lista över optimering flaggor</span><span class="sxs-lookup"><span data-stu-id="292cd-288">Comma-separated list of optimizer flags</span></span>|


<span data-ttu-id="292cd-289">Här följer några exempel på användning.</span><span class="sxs-lookup"><span data-stu-id="292cd-289">Here are some usage examples.</span></span>

<span data-ttu-id="292cd-290">Kompilera ett U-SQL-skript:</span><span class="sxs-lookup"><span data-stu-id="292cd-290">Compile a U-SQL script:</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql

<span data-ttu-id="292cd-291">Kompilera ett U-SQL-skript och ange data-rotmappen.</span><span class="sxs-lookup"><span data-stu-id="292cd-291">Compile a U-SQL script and set the data-root folder.</span></span> <span data-ttu-id="292cd-292">Observera att detta ersätter miljövariabeln uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="292cd-292">Note that this will overwrite the set environment variable.</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

<span data-ttu-id="292cd-293">Kompilera ett U-SQL-skript och ange en arbetskatalog och referenssammansättning databasen:</span><span class="sxs-lookup"><span data-stu-id="292cd-293">Compile a U-SQL script and set a working directory, reference assembly, and database:</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a><span data-ttu-id="292cd-294">Köra kompilerade resultat</span><span class="sxs-lookup"><span data-stu-id="292cd-294">Execute compiled results</span></span>

<span data-ttu-id="292cd-295">Den **köra** kommandot används för att köra kompilerade resultatet.</span><span class="sxs-lookup"><span data-stu-id="292cd-295">The **execute** command is used to execute compiled results.</span></span>   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

<span data-ttu-id="292cd-296">Följande är valfria argument för **köra**:</span><span class="sxs-lookup"><span data-stu-id="292cd-296">The following are optional arguments for **execute**:</span></span>

|<span data-ttu-id="292cd-297">Argumentet</span><span class="sxs-lookup"><span data-stu-id="292cd-297">Argument</span></span>|<span data-ttu-id="292cd-298">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="292cd-298">Description</span></span>|
|--------|-----------|
|<span data-ttu-id="292cd-299">-DataRoot [standardvärde '']</span><span class="sxs-lookup"><span data-stu-id="292cd-299">-DataRoot [default value '']</span></span>|<span data-ttu-id="292cd-300">Dataroten för körning av metadata.</span><span class="sxs-lookup"><span data-stu-id="292cd-300">Data root for metadata execution.</span></span> <span data-ttu-id="292cd-301">Används som standard den **LOCALRUN_DATAROOT** miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="292cd-301">It defaults to the **LOCALRUN_DATAROOT** environment variable.</span></span>|
|<span data-ttu-id="292cd-302">-MessageOut [standardvärde '']</span><span class="sxs-lookup"><span data-stu-id="292cd-302">-MessageOut [default value '']</span></span>|<span data-ttu-id="292cd-303">Dumpa meddelanden på konsolen till en fil.</span><span class="sxs-lookup"><span data-stu-id="292cd-303">Dump messages on the console to a file.</span></span>|
|<span data-ttu-id="292cd-304">-Parallell [standardvärdet '1']</span><span class="sxs-lookup"><span data-stu-id="292cd-304">-Parallel [default value '1']</span></span>|<span data-ttu-id="292cd-305">Indikator för att köra de genererade lokala kör steg med det angivna parallellitet.</span><span class="sxs-lookup"><span data-stu-id="292cd-305">Indicator to run the generated local-run steps with the specified parallelism level.</span></span>|
|<span data-ttu-id="292cd-306">-Verbose [standardvärde 'False']</span><span class="sxs-lookup"><span data-stu-id="292cd-306">-Verbose [default value 'False']</span></span>|<span data-ttu-id="292cd-307">Indikator för att visa detaljerade utdata från körning.</span><span class="sxs-lookup"><span data-stu-id="292cd-307">Indicator to show detailed outputs from runtime.</span></span>|

<span data-ttu-id="292cd-308">Här är ett exempel på användning:</span><span class="sxs-lookup"><span data-stu-id="292cd-308">Here's a usage example:</span></span>

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-the-sdk-with-programming-interfaces"></a><span data-ttu-id="292cd-309">Använd SDK: N med programmeringsgränssnitt</span><span class="sxs-lookup"><span data-stu-id="292cd-309">Use the SDK with programming interfaces</span></span>

<span data-ttu-id="292cd-310">Programmeringsgränssnitt finns i LocalRunHelper.exe.</span><span class="sxs-lookup"><span data-stu-id="292cd-310">The programming interfaces are all located in the LocalRunHelper.exe.</span></span> <span data-ttu-id="292cd-311">Du kan använda dem för att integrera funktionerna i SDK U-SQL och C# test framework för att skala din lokala test för U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="292cd-311">You can use them to integrate the functionality of the U-SQL SDK and the C# test framework to scale your U-SQL script local test.</span></span> <span data-ttu-id="292cd-312">I den här artikeln använder jag standard C# enhet test-projektet för att visa hur du använder dessa gränssnitt för att testa U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="292cd-312">In this article, I will use the standard C# unit test project to show how to use these interfaces to test your U-SQL script.</span></span>

### <a name="step-1-create-c-unit-test-project-and-configuration"></a><span data-ttu-id="292cd-313">Steg 1: Skapa C# enhet test-projekt och konfiguration</span><span class="sxs-lookup"><span data-stu-id="292cd-313">Step 1: Create C# unit test project and configuration</span></span>

- <span data-ttu-id="292cd-314">Skapa ett C# enhet testprojekt via fil > Nytt > Projekt > Visual C# > Testa > enhet Testa projektet.</span><span class="sxs-lookup"><span data-stu-id="292cd-314">Create a C# unit test project through File > New > Project > Visual C# > Test > Unit Test Project.</span></span>
- <span data-ttu-id="292cd-315">Lägg till LocalRunHelper.exe som en referens för projektet.</span><span class="sxs-lookup"><span data-stu-id="292cd-315">Add LocalRunHelper.exe as a reference for the project.</span></span> <span data-ttu-id="292cd-316">LocalRunHelper.exe finns i \build\runtime\LocalRunHelper.exe i Nuget-paketet.</span><span class="sxs-lookup"><span data-stu-id="292cd-316">The LocalRunHelper.exe is located at \build\runtime\LocalRunHelper.exe in Nuget package.</span></span>

    ![Azure Data Lake U-SQL-SDK Lägg till referens](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- <span data-ttu-id="292cd-318">U-SQL-SDK **endast** x64 Stödmiljö, se till att ange build platform mål som x64.</span><span class="sxs-lookup"><span data-stu-id="292cd-318">U-SQL SDK **only** support x64 environment, make sure to set build platform target as x64.</span></span> <span data-ttu-id="292cd-319">Du kan ange som via projektegenskapen > Skapa > plattform mål.</span><span class="sxs-lookup"><span data-stu-id="292cd-319">You can set that through Project Property > Build > Platform target.</span></span>

    ![Azure Data Lake U-SQL-SDK konfigurera x64 projekt](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- <span data-ttu-id="292cd-321">Se till att ange din testmiljö som x64.</span><span class="sxs-lookup"><span data-stu-id="292cd-321">Make sure to set your test environment as x64.</span></span> <span data-ttu-id="292cd-322">I Visual Studio kan du ändra det genom testning > Testa inställningar > standard processorarkitektur > x64.</span><span class="sxs-lookup"><span data-stu-id="292cd-322">In Visual Studio, you can set it through Test > Test Settings > Default Processor Architecture > x64.</span></span>

    ![Azure Data Lake U-SQL-SDK konfigurera x64 testmiljö](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- <span data-ttu-id="292cd-324">Se till att kopiera alla beroende filer under NugetPackage\build\runtime\ till projektet arbetskatalog som vanligtvis är under ProjectFolder\bin\x64\Debug.</span><span class="sxs-lookup"><span data-stu-id="292cd-324">Make sure to copy all dependency files under NugetPackage\build\runtime\ to project working directory which is usually under ProjectFolder\bin\x64\Debug.</span></span>

### <a name="step-2-create-u-sql-script-test-case"></a><span data-ttu-id="292cd-325">Steg 2: Skapa testfall för U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="292cd-325">Step 2: Create U-SQL script test case</span></span>

<span data-ttu-id="292cd-326">Nedan visas exempelkod för U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="292cd-326">Below is the sample code for U-SQL script test.</span></span> <span data-ttu-id="292cd-327">För att testa måste du förbereda skript indata och utdata som förväntas-filer.</span><span class="sxs-lookup"><span data-stu-id="292cd-327">For testing, you need to prepare scripts, input files and expected output files.</span></span>

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
                //Specify the local run message output path
                StreamWriter MessageOutput = new StreamWriter("../../../log.txt");

                LocalRunHelper localrun = new LocalRunHelper(MessageOutput);

                //Configure the DateRoot path, Script Path and CPPSDK path
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

                //Don't forget to close MessageOutput to get logs into file
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


### <a name="programming-interfaces-in-localrunhelperexe"></a><span data-ttu-id="292cd-328">Programmeringsgränssnitt i LocalRunHelper.exe</span><span class="sxs-lookup"><span data-stu-id="292cd-328">Programming interfaces in LocalRunHelper.exe</span></span>

<span data-ttu-id="292cd-329">LocalRunHelper.exe ger programmeringsgränssnitt för U-SQL lokalt kompilera kör, osv. Gränssnitt som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="292cd-329">LocalRunHelper.exe provides the programming interfaces for U-SQL local compile, run, etc. The interfaces are listed as follows.</span></span>

<span data-ttu-id="292cd-330">**Konstruktorn**</span><span class="sxs-lookup"><span data-stu-id="292cd-330">**Constructor**</span></span>

<span data-ttu-id="292cd-331">offentliga LocalRunHelper ([System.IO.TextWriter messageOutput = null])</span><span class="sxs-lookup"><span data-stu-id="292cd-331">public LocalRunHelper([System.IO.TextWriter messageOutput = null])</span></span>

|<span data-ttu-id="292cd-332">Parameter</span><span class="sxs-lookup"><span data-stu-id="292cd-332">Parameter</span></span>|<span data-ttu-id="292cd-333">Typ</span><span class="sxs-lookup"><span data-stu-id="292cd-333">Type</span></span>|<span data-ttu-id="292cd-334">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="292cd-334">Description</span></span>|
|---------|----|-----------|
|<span data-ttu-id="292cd-335">messageOutput</span><span class="sxs-lookup"><span data-stu-id="292cd-335">messageOutput</span></span>|<span data-ttu-id="292cd-336">System.IO.TextWriter</span><span class="sxs-lookup"><span data-stu-id="292cd-336">System.IO.TextWriter</span></span>|<span data-ttu-id="292cd-337">Ange null-konsolen för utgående meddelanden.</span><span class="sxs-lookup"><span data-stu-id="292cd-337">for output messages, set to null to use Console</span></span>|

<span data-ttu-id="292cd-338">**Egenskaper**</span><span class="sxs-lookup"><span data-stu-id="292cd-338">**Properties**</span></span>

|<span data-ttu-id="292cd-339">Egenskap</span><span class="sxs-lookup"><span data-stu-id="292cd-339">Property</span></span>|<span data-ttu-id="292cd-340">Typ</span><span class="sxs-lookup"><span data-stu-id="292cd-340">Type</span></span>|<span data-ttu-id="292cd-341">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="292cd-341">Description</span></span>|
|--------|----|-----------|
|<span data-ttu-id="292cd-342">AlgebraPath</span><span class="sxs-lookup"><span data-stu-id="292cd-342">AlgebraPath</span></span>|<span data-ttu-id="292cd-343">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-343">string</span></span>|<span data-ttu-id="292cd-344">Sökvägen till filen algebra (algebra filen är ett resultat för kompilering)</span><span class="sxs-lookup"><span data-stu-id="292cd-344">The path to algebra file (algebra file is one of the compilation results)</span></span>|
|<span data-ttu-id="292cd-345">CodeBehindReferences</span><span class="sxs-lookup"><span data-stu-id="292cd-345">CodeBehindReferences</span></span>|<span data-ttu-id="292cd-346">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-346">string</span></span>|<span data-ttu-id="292cd-347">Om skriptet har ytterligare kod bakom referenser, ange sökvägar avgränsade med ';'</span><span class="sxs-lookup"><span data-stu-id="292cd-347">If the script has additional code behind references, specify the paths separated with ';'</span></span>|
|<span data-ttu-id="292cd-348">CppSdkDir</span><span class="sxs-lookup"><span data-stu-id="292cd-348">CppSdkDir</span></span>|<span data-ttu-id="292cd-349">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-349">string</span></span>|<span data-ttu-id="292cd-350">CppSDK directory</span><span class="sxs-lookup"><span data-stu-id="292cd-350">CppSDK directory</span></span>|
|<span data-ttu-id="292cd-351">CurrentDir</span><span class="sxs-lookup"><span data-stu-id="292cd-351">CurrentDir</span></span>|<span data-ttu-id="292cd-352">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-352">string</span></span>|<span data-ttu-id="292cd-353">Aktuell katalog</span><span class="sxs-lookup"><span data-stu-id="292cd-353">Current directory</span></span>|
|<span data-ttu-id="292cd-354">DataRoot</span><span class="sxs-lookup"><span data-stu-id="292cd-354">DataRoot</span></span>|<span data-ttu-id="292cd-355">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-355">string</span></span>|<span data-ttu-id="292cd-356">Rotsökvägen för data</span><span class="sxs-lookup"><span data-stu-id="292cd-356">Data root path</span></span>|
|<span data-ttu-id="292cd-357">DebuggerMailPath</span><span class="sxs-lookup"><span data-stu-id="292cd-357">DebuggerMailPath</span></span>|<span data-ttu-id="292cd-358">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-358">string</span></span>|<span data-ttu-id="292cd-359">Sökvägen till felsökare mailslot</span><span class="sxs-lookup"><span data-stu-id="292cd-359">The path to debugger mailslot</span></span>|
|<span data-ttu-id="292cd-360">GenerateUdoRedirect</span><span class="sxs-lookup"><span data-stu-id="292cd-360">GenerateUdoRedirect</span></span>|<span data-ttu-id="292cd-361">bool</span><span class="sxs-lookup"><span data-stu-id="292cd-361">bool</span></span>|<span data-ttu-id="292cd-362">Om vi vill generera sammansättningen som lästes in omdirigering åsidosätta config</span><span class="sxs-lookup"><span data-stu-id="292cd-362">If we want to generate assembly loading redirection override config</span></span>|
|<span data-ttu-id="292cd-363">HasCodeBehind</span><span class="sxs-lookup"><span data-stu-id="292cd-363">HasCodeBehind</span></span>|<span data-ttu-id="292cd-364">bool</span><span class="sxs-lookup"><span data-stu-id="292cd-364">bool</span></span>|<span data-ttu-id="292cd-365">Om skriptet har bakomliggande kod</span><span class="sxs-lookup"><span data-stu-id="292cd-365">If the script has code behind</span></span>|
|<span data-ttu-id="292cd-366">InputDir</span><span class="sxs-lookup"><span data-stu-id="292cd-366">InputDir</span></span>|<span data-ttu-id="292cd-367">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-367">string</span></span>|<span data-ttu-id="292cd-368">Katalogen för indata</span><span class="sxs-lookup"><span data-stu-id="292cd-368">Directory for input data</span></span>|
|<span data-ttu-id="292cd-369">MessagePath</span><span class="sxs-lookup"><span data-stu-id="292cd-369">MessagePath</span></span>|<span data-ttu-id="292cd-370">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-370">string</span></span>|<span data-ttu-id="292cd-371">Meddelandet dump sökväg</span><span class="sxs-lookup"><span data-stu-id="292cd-371">Message dump file path</span></span>|
|<span data-ttu-id="292cd-372">OutputDir</span><span class="sxs-lookup"><span data-stu-id="292cd-372">OutputDir</span></span>|<span data-ttu-id="292cd-373">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-373">string</span></span>|<span data-ttu-id="292cd-374">Katalogen för utdata</span><span class="sxs-lookup"><span data-stu-id="292cd-374">Directory for output data</span></span>|
|<span data-ttu-id="292cd-375">Parallellitet</span><span class="sxs-lookup"><span data-stu-id="292cd-375">Parallelism</span></span>|<span data-ttu-id="292cd-376">int</span><span class="sxs-lookup"><span data-stu-id="292cd-376">int</span></span>|<span data-ttu-id="292cd-377">Parallellitet att köra algebra</span><span class="sxs-lookup"><span data-stu-id="292cd-377">Parallelism to run the algebra</span></span>|
|<span data-ttu-id="292cd-378">ParentPid</span><span class="sxs-lookup"><span data-stu-id="292cd-378">ParentPid</span></span>|<span data-ttu-id="292cd-379">int</span><span class="sxs-lookup"><span data-stu-id="292cd-379">int</span></span>|<span data-ttu-id="292cd-380">Process-ID för den överordnade där tjänsten övervakar om du vill avsluta, värdet 0 eller ett negativt om du vill ignorera</span><span class="sxs-lookup"><span data-stu-id="292cd-380">PID of the parent on which the service monitors to exit, set to 0 or negative to ignore</span></span>|
|<span data-ttu-id="292cd-381">ResultPath</span><span class="sxs-lookup"><span data-stu-id="292cd-381">ResultPath</span></span>|<span data-ttu-id="292cd-382">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-382">string</span></span>|<span data-ttu-id="292cd-383">Resultatet dump sökväg</span><span class="sxs-lookup"><span data-stu-id="292cd-383">Result dump file path</span></span>|
|<span data-ttu-id="292cd-384">RuntimeDir</span><span class="sxs-lookup"><span data-stu-id="292cd-384">RuntimeDir</span></span>|<span data-ttu-id="292cd-385">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-385">string</span></span>|<span data-ttu-id="292cd-386">Runtime directory</span><span class="sxs-lookup"><span data-stu-id="292cd-386">Runtime directory</span></span>|
|<span data-ttu-id="292cd-387">ScriptPath</span><span class="sxs-lookup"><span data-stu-id="292cd-387">ScriptPath</span></span>|<span data-ttu-id="292cd-388">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-388">string</span></span>|<span data-ttu-id="292cd-389">Var du hittar skriptet</span><span class="sxs-lookup"><span data-stu-id="292cd-389">Where to find the script</span></span>|
|<span data-ttu-id="292cd-390">Lite</span><span class="sxs-lookup"><span data-stu-id="292cd-390">Shallow</span></span>|<span data-ttu-id="292cd-391">bool</span><span class="sxs-lookup"><span data-stu-id="292cd-391">bool</span></span>|<span data-ttu-id="292cd-392">Ytlig kompilera eller inte</span><span class="sxs-lookup"><span data-stu-id="292cd-392">Shallow compile or not</span></span>|
|<span data-ttu-id="292cd-393">TempDir</span><span class="sxs-lookup"><span data-stu-id="292cd-393">TempDir</span></span>|<span data-ttu-id="292cd-394">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-394">string</span></span>|<span data-ttu-id="292cd-395">Temp-katalogen</span><span class="sxs-lookup"><span data-stu-id="292cd-395">Temp directory</span></span>|
|<span data-ttu-id="292cd-396">UseDataBase</span><span class="sxs-lookup"><span data-stu-id="292cd-396">UseDataBase</span></span>|<span data-ttu-id="292cd-397">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-397">string</span></span>|<span data-ttu-id="292cd-398">Ange databasen som ska användas för koden bakom tillfällig sammansättning registrering, master som standard</span><span class="sxs-lookup"><span data-stu-id="292cd-398">Specify the database to use for code behind temporary assembly registration, master by default</span></span>|
|<span data-ttu-id="292cd-399">WorkDir</span><span class="sxs-lookup"><span data-stu-id="292cd-399">WorkDir</span></span>|<span data-ttu-id="292cd-400">Sträng</span><span class="sxs-lookup"><span data-stu-id="292cd-400">string</span></span>|<span data-ttu-id="292cd-401">Prioriterade arbetskatalogen</span><span class="sxs-lookup"><span data-stu-id="292cd-401">Preferred working directory</span></span>|


<span data-ttu-id="292cd-402">**Metoden**</span><span class="sxs-lookup"><span data-stu-id="292cd-402">**Method**</span></span>

|<span data-ttu-id="292cd-403">Metod</span><span class="sxs-lookup"><span data-stu-id="292cd-403">Method</span></span>|<span data-ttu-id="292cd-404">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="292cd-404">Description</span></span>|<span data-ttu-id="292cd-405">Returnera</span><span class="sxs-lookup"><span data-stu-id="292cd-405">Return</span></span>|<span data-ttu-id="292cd-406">Parameter</span><span class="sxs-lookup"><span data-stu-id="292cd-406">Parameter</span></span>|
|------|-----------|------|---------|
|<span data-ttu-id="292cd-407">offentliga bool DoCompile()</span><span class="sxs-lookup"><span data-stu-id="292cd-407">public bool DoCompile()</span></span>|<span data-ttu-id="292cd-408">Kompilera U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="292cd-408">Compile the U-SQL script</span></span>|<span data-ttu-id="292cd-409">SANT lyckades</span><span class="sxs-lookup"><span data-stu-id="292cd-409">True on success</span></span>| |
|<span data-ttu-id="292cd-410">offentliga bool DoExec()</span><span class="sxs-lookup"><span data-stu-id="292cd-410">public bool DoExec()</span></span>|<span data-ttu-id="292cd-411">Köra kompilerade resultatet</span><span class="sxs-lookup"><span data-stu-id="292cd-411">Execute the compiled result</span></span>|<span data-ttu-id="292cd-412">SANT lyckades</span><span class="sxs-lookup"><span data-stu-id="292cd-412">True on success</span></span>| |
|<span data-ttu-id="292cd-413">offentliga bool DoRun()</span><span class="sxs-lookup"><span data-stu-id="292cd-413">public bool DoRun()</span></span>|<span data-ttu-id="292cd-414">Kör U-SQL-skript (kompilera + Execute)</span><span class="sxs-lookup"><span data-stu-id="292cd-414">Run the U-SQL script (Compile + Execute)</span></span>|<span data-ttu-id="292cd-415">SANT lyckades</span><span class="sxs-lookup"><span data-stu-id="292cd-415">True on success</span></span>| |
|<span data-ttu-id="292cd-416">offentliga bool IsValidRuntimeDir (sträng sökväg)</span><span class="sxs-lookup"><span data-stu-id="292cd-416">public bool IsValidRuntimeDir(string path)</span></span>|<span data-ttu-id="292cd-417">Kontrollera om den angivna sökvägen är giltig runtime sökväg</span><span class="sxs-lookup"><span data-stu-id="292cd-417">Check if the given path is valid runtime path</span></span>|<span data-ttu-id="292cd-418">Sant för giltigt</span><span class="sxs-lookup"><span data-stu-id="292cd-418">True for valid</span></span>|<span data-ttu-id="292cd-419">Sökvägen till katalogen för körning</span><span class="sxs-lookup"><span data-stu-id="292cd-419">The path of runtime directory</span></span>|


## <a name="faq-about-common-issue"></a><span data-ttu-id="292cd-420">Vanliga frågor och svar om vanliga problem</span><span class="sxs-lookup"><span data-stu-id="292cd-420">FAQ about common issue</span></span>

### <a name="error-1"></a><span data-ttu-id="292cd-421">Fel 1:</span><span class="sxs-lookup"><span data-stu-id="292cd-421">Error 1:</span></span>
<span data-ttu-id="292cd-422">E_CSC_SYSTEM_INTERNAL: Internt fel!</span><span class="sxs-lookup"><span data-stu-id="292cd-422">E_CSC_SYSTEM_INTERNAL: Internal error!</span></span> <span data-ttu-id="292cd-423">Det gick inte att läsa in filen eller sammansättningen 'ScopeEngineManaged.dll' eller en av dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="292cd-423">Could not load file or assembly 'ScopeEngineManaged.dll' or one of its dependencies.</span></span> <span data-ttu-id="292cd-424">Det gick inte att hitta den angivna modulen.</span><span class="sxs-lookup"><span data-stu-id="292cd-424">The specified module could not be found.</span></span>

<span data-ttu-id="292cd-425">Kontrollera följande:</span><span class="sxs-lookup"><span data-stu-id="292cd-425">Please check the following:</span></span>

- <span data-ttu-id="292cd-426">Kontrollera att du har x64 miljö.</span><span class="sxs-lookup"><span data-stu-id="292cd-426">Make sure you have x64 environment.</span></span> <span data-ttu-id="292cd-427">Build-målplattform och testmiljön ska vara x64, referera till **steg 1: skapa C# enhet Testa projektet och konfiguration** ovan.</span><span class="sxs-lookup"><span data-stu-id="292cd-427">The build target platform and the test environment should be x64, refer to **Step 1: Create C# unit test project and configuration** above.</span></span>
- <span data-ttu-id="292cd-428">Kontrollera att du har kopierat alla beroende filer under NugetPackage\build\runtime\ till projektet arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="292cd-428">Make sure you have copied all dependency files under NugetPackage\build\runtime\ to project working directory.</span></span>


## <a name="next-steps"></a><span data-ttu-id="292cd-429">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="292cd-429">Next steps</span></span>

* <span data-ttu-id="292cd-430">Information om U-SQL finns i [Kom igång med U-SQL-språk i Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="292cd-430">To learn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="292cd-431">Om du vill logga diagnostikinformation, se [åtkomst till diagnostik loggar för Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="292cd-431">To log diagnostics information, see [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).</span></span>
* <span data-ttu-id="292cd-432">Om du vill se en mer komplex fråga, se [analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="292cd-432">To see a more complex query, see [Analyze website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="292cd-433">Om du vill visa jobbinformation [Använd jobbet webbläsare och visa jobb för Azure Data Lake Analytics-jobb](data-lake-analytics-data-lake-tools-view-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="292cd-433">To view job details, see [Use Job Browser and Job View for Azure Data Lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md).</span></span>
* <span data-ttu-id="292cd-434">Om du vill använda vyn vertex körning finns [använda vyn Vertex körning i Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span><span class="sxs-lookup"><span data-stu-id="292cd-434">To use the vertex execution view, see [Use the Vertex Execution View in Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span></span>
