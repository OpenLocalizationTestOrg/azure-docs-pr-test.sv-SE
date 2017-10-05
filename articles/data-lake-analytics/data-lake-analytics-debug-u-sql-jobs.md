---
title: "Felsöka U-SQL-jobb | Microsoft Docs"
description: "Lär dig att felsöka en misslyckad U-SQL-nod med hjälp av Visual Studio."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/02/2016
ms.author: saveenr
ms.openlocfilehash: 2a77c72d3062272305208934d6406d040266c753
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a><span data-ttu-id="01d32-103">Felsöka användardefinierade C#-kod för misslyckade U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="01d32-103">Debug user-defined C# code for failed U-SQL jobs</span></span>

<span data-ttu-id="01d32-104">U-SQL ger en modellen för utökning med C#, så att du kan skriva koden för att lägga till funktioner, till exempel en anpassad extraktor eller reducer.</span><span class="sxs-lookup"><span data-stu-id="01d32-104">U-SQL provides an extensibility model using C#, so you can write your code to add functionality such as a custom extractor or reducer.</span></span> <span data-ttu-id="01d32-105">Läs mer i [U-SQL-programmering guiden](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf).</span><span class="sxs-lookup"><span data-stu-id="01d32-105">To learn more, see [U-SQL programmability guide](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf).</span></span> <span data-ttu-id="01d32-106">I praktiken någon kod måste felsökning och ge endast system för stordata begränsad runtime felsökningsinformation som loggfiler.</span><span class="sxs-lookup"><span data-stu-id="01d32-106">In practice any code may need debugging, and big data systems may only provide limited runtime debugging information such as log files.</span></span>

<span data-ttu-id="01d32-107">Azure Data Lake-verktyg för Visual Studio innehåller en funktion som kallas **misslyckades Vertex felsöka**, vilket gör att du klona en misslyckade jobbet från molnet till den lokala datorn för felsökning.</span><span class="sxs-lookup"><span data-stu-id="01d32-107">Azure Data Lake Tools for Visual Studio provides a feature called **Failed Vertex Debug**, which lets you clone a failed job from the cloud to your local machine for debugging.</span></span> <span data-ttu-id="01d32-108">Lokala klonen fångar hela molnmiljön, inklusive alla indata och användarkod.</span><span class="sxs-lookup"><span data-stu-id="01d32-108">The local clone captures the entire cloud environment, including any input data and user code.</span></span>

<span data-ttu-id="01d32-109">Följande videoklipp visar misslyckades Vertex Debug i Azure Data Lake-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01d32-109">The following video demonstrates Failed Vertex Debug in Azure Data Lake Tools for Visual Studio.</span></span>

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>

> [!NOTE]
> <span data-ttu-id="01d32-110">Visual Studio kräver följande två uppdateringar, om de inte redan är installerade: [Microsoft Visual C++ 2015 Redistributable uppdatering 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) och [Universal C Runtime för Windows](https://www.microsoft.com/download/details.aspx?id=50410).</span><span class="sxs-lookup"><span data-stu-id="01d32-110">Visual Studio requires the following two updates, if they are not already installed: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) and the [Universal C Runtime for Windows](https://www.microsoft.com/download/details.aspx?id=50410).</span></span>

## <a name="download-failed-vertex-to-local-machine"></a><span data-ttu-id="01d32-111">Gick inte att hämta hörn till den lokala datorn</span><span class="sxs-lookup"><span data-stu-id="01d32-111">Download failed vertex to local machine</span></span>

<span data-ttu-id="01d32-112">När du öppnar ett jobb som misslyckades i Azure Data Lake-verktyg för Visual Studio kan se du ett gult avisering fält med detaljerade felmeddelanden på fliken fel.</span><span class="sxs-lookup"><span data-stu-id="01d32-112">When you open a failed job in Azure Data Lake Tools for Visual Studio, you see a yellow alert bar with detailed error messages in the error tab.</span></span>

1. <span data-ttu-id="01d32-113">Klicka på **hämta** att hämta alla nödvändiga resurser och inkommande dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="01d32-113">Click **Download** to download all the required resources and input streams.</span></span> <span data-ttu-id="01d32-114">Om hämtningen inte slutförs, klicka på **försök**.</span><span class="sxs-lookup"><span data-stu-id="01d32-114">If the download doesn't complete, click **Retry**.</span></span>

2. <span data-ttu-id="01d32-115">Klicka på **öppna** När nedladdningen är klar för att generera en lokal felsökningsmiljö.</span><span class="sxs-lookup"><span data-stu-id="01d32-115">Click **Open** after the download completes to generate a local debugging environment.</span></span> <span data-ttu-id="01d32-116">En ny Visual Studio-instans med en lösning för felsökning skapas och öppnas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="01d32-116">A new Visual Studio instance with a debugging solution is automatically created and opened.</span></span>

![Azure Data Lake Analytics U-SQL felsökning i visual studio download vertex](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

<span data-ttu-id="01d32-118">Jobb kan inkludera bakomliggande kod källfiler eller registrerade sammansättningar, och dessa två typer har olika scenarier för felsökning.</span><span class="sxs-lookup"><span data-stu-id="01d32-118">Jobs may include code-behind source files or registered assemblies, and these two types have different debugging scenarios.</span></span>

- [<span data-ttu-id="01d32-119">Felsöka ett jobb som misslyckades med bakomliggande kod</span><span class="sxs-lookup"><span data-stu-id="01d32-119">Debug a failed job with code-behind</span></span>](#debug-job-failed-with-code-behind)
- [<span data-ttu-id="01d32-120">Felsöka ett jobb som misslyckades med sammansättningar</span><span class="sxs-lookup"><span data-stu-id="01d32-120">Debug a failed job with assemblies</span></span>](#debug-job-failed-with-assemblies)


## <a name="debug-job-failed-with-code-behind"></a><span data-ttu-id="01d32-121">Felsöka jobb som misslyckades med bakomliggande kod</span><span class="sxs-lookup"><span data-stu-id="01d32-121">Debug job failed with code-behind</span></span>

<span data-ttu-id="01d32-122">Om ett U-SQL-jobb misslyckas och jobbet innehåller användarkod (vanligtvis namnet `Script.usql.cs` i ett U-SQL-projekt), att källkoden har importerats till felsökning lösningen.</span><span class="sxs-lookup"><span data-stu-id="01d32-122">If a U-SQL job fails, and the job includes user code (typically named `Script.usql.cs` in a U-SQL project), that source code is imported into the debugging solution.</span></span>  <span data-ttu-id="01d32-123">Därifrån kan du använda Visual Studio felsökningsverktyg (titta på, variabler, etc.) för att felsöka problemet.</span><span class="sxs-lookup"><span data-stu-id="01d32-123">From there you can use the Visual Studio debugging tools (watch, variables, etc.) to troubleshoot the problem.</span></span>

> [!NOTE]
> <span data-ttu-id="01d32-124">Innan du felsökning, måste du kontrollera att **Common Language Runtime undantag** i fönstret undantag inställningar (**Ctrl + Alt + E**).</span><span class="sxs-lookup"><span data-stu-id="01d32-124">Before debugging, be sure to check **Common Language Runtime Exceptions** in the Exception Settings window (**Ctrl + Alt + E**).</span></span>

![Azure Data Lake Analytics U-SQL felsökning i visual studio-inställning](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. <span data-ttu-id="01d32-126">Tryck på **F5** att köra kod bakomliggande kod.</span><span class="sxs-lookup"><span data-stu-id="01d32-126">Press **F5** to run the code-behind code.</span></span> <span data-ttu-id="01d32-127">Det kan köras förrän den har stoppats av ett undantagsfel.</span><span class="sxs-lookup"><span data-stu-id="01d32-127">It will run until it is stopped by an exception.</span></span>

2. <span data-ttu-id="01d32-128">Öppna den `ADLTool_Codebehind.usql.cs` filen och ange brytpunkter, tryck på **F5** för felsökning av kod steg för steg.</span><span class="sxs-lookup"><span data-stu-id="01d32-128">Open the `ADLTool_Codebehind.usql.cs` file and set breakpoints, then press **F5** to debug the code step by step.</span></span>

    ![Azure Data Lake Analytics U-SQL debug-undantag](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a><span data-ttu-id="01d32-130">Felsöka jobb som misslyckades med sammansättningar</span><span class="sxs-lookup"><span data-stu-id="01d32-130">Debug job failed with assemblies</span></span>

<span data-ttu-id="01d32-131">Om du använder registrerade sammansättningar i U-SQL-skript kan systemet kan inte hämta källkoden automatiskt.</span><span class="sxs-lookup"><span data-stu-id="01d32-131">If you use registered assemblies in your U-SQL script, the system can't get the source code automatically.</span></span> <span data-ttu-id="01d32-132">I det här fallet manuellt lägger till de sammansättningarna källfilerna i lösningen.</span><span class="sxs-lookup"><span data-stu-id="01d32-132">In this case, manually add the assemblies' source code files to the solution.</span></span>

### <a name="configure-the-solution"></a><span data-ttu-id="01d32-133">Konfigurera lösningen</span><span class="sxs-lookup"><span data-stu-id="01d32-133">Configure the solution</span></span>

1. <span data-ttu-id="01d32-134">Högerklicka på **lösning 'VertexDebug' > Lägg till > befintligt projekt...**  att hitta de sammansättningarna källkoden och lägger till projektet i Felsökning lösningen.</span><span class="sxs-lookup"><span data-stu-id="01d32-134">Right-click **Solution 'VertexDebug' > Add > Existing Project...** to find the assemblies' source code and add the project to the debugging solution.</span></span>

    ![Azure Data Lake Analytics U-SQL-debug lägga till projekt](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. <span data-ttu-id="01d32-136">Högerklicka på **LocalVertexHost > Egenskaper** i lösningen och kopiera den **Working Directory** sökväg.</span><span class="sxs-lookup"><span data-stu-id="01d32-136">Right-click **LocalVertexHost > Properties** in the solution and copy the **Working Directory** path.</span></span>

3. <span data-ttu-id="01d32-137">Högerklicka på **sammansättningsprojekt källa kod > Egenskaper**väljer den **skapa** fliken längst till vänster och klistra in den kopierade sökvägen som **utdata > Utdatasökvägen**.</span><span class="sxs-lookup"><span data-stu-id="01d32-137">Right-Click **assembly source code project > Properties**, select the **Build** tab at left, and paste the copied path as **Output > Output path**.</span></span>

    ![Ange sökväg till pdb för Azure Data Lake Analytics U-SQL-felsökning](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. <span data-ttu-id="01d32-139">Tryck på **Ctrl + Alt + E**, kontrollera **Common Language Runtime undantag** i inställningar för undantag.</span><span class="sxs-lookup"><span data-stu-id="01d32-139">Press **Ctrl + Alt + E**, check **Common Language Runtime Exceptions** in Exception Settings window.</span></span>

### <a name="start-debug"></a><span data-ttu-id="01d32-140">Starta felsökning</span><span class="sxs-lookup"><span data-stu-id="01d32-140">Start debug</span></span>

1. <span data-ttu-id="01d32-141">Högerklicka på **Kodprojekt för sammansättningen källa > återskapa** till .pdb utdatafilerna till den `LocalVertexHost` arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="01d32-141">Right-click **assembly source code project > Rebuild** to output .pdb files to the `LocalVertexHost` working directory.</span></span>

2. <span data-ttu-id="01d32-142">Tryck på **F5** och projektet kommer att köras tills den har stoppats av ett undantagsfel.</span><span class="sxs-lookup"><span data-stu-id="01d32-142">Press **F5** and the project will run until it is stopped by an exception.</span></span> <span data-ttu-id="01d32-143">Du kan se följande varningsmeddelande som du kan ignorera.</span><span class="sxs-lookup"><span data-stu-id="01d32-143">You may see the following warning message, which you can safely ignore.</span></span> <span data-ttu-id="01d32-144">Det kan ta upp till en minut att komma till skärmen för felsökning.</span><span class="sxs-lookup"><span data-stu-id="01d32-144">It can take up to a minute to get to the debug screen.</span></span>

    ![Azure Data Lake Analytics U-SQL felsökning i visual studio-varning](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. <span data-ttu-id="01d32-146">Öppna källkoden och ange brytpunkter, tryck på **F5** för felsökning av kod steg för steg.</span><span class="sxs-lookup"><span data-stu-id="01d32-146">Open your source code and set breakpoints, then press **F5** to debug the code step by step.</span></span>

<span data-ttu-id="01d32-147">Du kan också använda Visual Studio felsökningsverktyg (titta på, variabler, etc.) för att felsöka problemet.</span><span class="sxs-lookup"><span data-stu-id="01d32-147">You can also use the Visual Studio debugging tools (watch, variables, etc.) to troubleshoot the problem.</span></span>

> [!NOTE]
> <span data-ttu-id="01d32-148">Återskapa sammansättningsprojekt källa kod varje gång när du ändrar koden för att generera uppdaterade .pdb filer.</span><span class="sxs-lookup"><span data-stu-id="01d32-148">Rebuild the assembly source code project each time after you modify the code to generate updated .pdb files.</span></span>

<span data-ttu-id="01d32-149">Efter felsökning visar utdatafönstret följande meddelande om projektet har slutförts:</span><span class="sxs-lookup"><span data-stu-id="01d32-149">After debugging, if the project completes successfully the output window shows the following message:</span></span>

```
The Program 'LocalVertexHost.exe' has exited with code 0 (0x0).
```

![Azure Data Lake Analytics U-SQL-debug lyckas](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-the-job"></a><span data-ttu-id="01d32-151">Skicka jobbet</span><span class="sxs-lookup"><span data-stu-id="01d32-151">Resubmit the job</span></span>

<span data-ttu-id="01d32-152">När du har slutfört felsökning, skicka om det misslyckade jobbet.</span><span class="sxs-lookup"><span data-stu-id="01d32-152">Once you have completed debugging, resubmit the failed job.</span></span>

1. <span data-ttu-id="01d32-153">Kopiera C#-koden till bakomliggande kod källfilen för jobb med bakomliggande kod lösningar (vanligtvis `Script.usql.cs`).</span><span class="sxs-lookup"><span data-stu-id="01d32-153">For jobs with code-behind solutions, copy your C# code into the code-behind source file (typically `Script.usql.cs`).</span></span>
2. <span data-ttu-id="01d32-154">Registrera uppdaterade .dll sammansättningarna i databasen ADLA för jobb med sammansättningar:</span><span class="sxs-lookup"><span data-stu-id="01d32-154">For jobs with assemblies, register the updated .dll assemblies into your ADLA database:</span></span>
    1. <span data-ttu-id="01d32-155">Från Server Explorer eller Cloud Explorer, expandera den **ADLA konto > databaser** nod.</span><span class="sxs-lookup"><span data-stu-id="01d32-155">From Server Explorer or Cloud Explorer, expand the **ADLA account > Databases** node.</span></span>
    2. <span data-ttu-id="01d32-156">Högerklicka på **sammansättningar** och registrera ditt nya .dll-sammansättningar med databasen ADLA: ![Azure Data Lake Analytics U-SQL-debug registrera sammansättningen](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)</span><span class="sxs-lookup"><span data-stu-id="01d32-156">Right-click **Assemblies** and register your new .dll assemblies with the ADLA database: ![Azure Data Lake Analytics U-SQL debug register assembly](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)</span></span>
3. <span data-ttu-id="01d32-157">Skicka jobbet.</span><span class="sxs-lookup"><span data-stu-id="01d32-157">Resubmit your job.</span></span>

## <a name="next-steps"></a><span data-ttu-id="01d32-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="01d32-158">Next steps</span></span>

- [<span data-ttu-id="01d32-159">U-SQL-programmering guide</span><span class="sxs-lookup"><span data-stu-id="01d32-159">U-SQL programmability guide</span></span>](data-lake-analytics-u-sql-programmability-guide.md)
- [<span data-ttu-id="01d32-160">Utveckla U-SQL-användardefinierade operatorer för Azure Data Lake Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="01d32-160">Develop U-SQL User-defined operators for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [<span data-ttu-id="01d32-161">Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01d32-161">Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
