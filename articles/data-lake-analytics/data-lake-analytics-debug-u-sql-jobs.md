---
title: aaaDebug U-SQL-jobb | Microsoft Docs
description: "Lär dig hur toodebug U-SQL misslyckades vertex med Visual Studio."
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
ms.openlocfilehash: 092bffa1a59ed91c5837402d0276447480b923fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a><span data-ttu-id="8eae3-103">Felsöka användardefinierade C#-kod för misslyckade U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="8eae3-103">Debug user-defined C# code for failed U-SQL jobs</span></span>

<span data-ttu-id="8eae3-104">U-SQL ger en modellen för utökning med C#, så att du kan skriva din kod tooadd funktioner, till exempel en anpassad extraktor eller reducer.</span><span class="sxs-lookup"><span data-stu-id="8eae3-104">U-SQL provides an extensibility model using C#, so you can write your code tooadd functionality such as a custom extractor or reducer.</span></span> <span data-ttu-id="8eae3-105">Det finns fler toolearn [U-SQL-programmering guiden](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf).</span><span class="sxs-lookup"><span data-stu-id="8eae3-105">toolearn more, see [U-SQL programmability guide](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf).</span></span> <span data-ttu-id="8eae3-106">I praktiken någon kod måste felsökning och ge endast system för stordata begränsad runtime felsökningsinformation som loggfiler.</span><span class="sxs-lookup"><span data-stu-id="8eae3-106">In practice any code may need debugging, and big data systems may only provide limited runtime debugging information such as log files.</span></span>

<span data-ttu-id="8eae3-107">Azure Data Lake-verktyg för Visual Studio innehåller en funktion som kallas **misslyckades Vertex felsöka**, vilket gör att du klona en misslyckade jobbet från hello molnet tooyour lokal dator för felsökning.</span><span class="sxs-lookup"><span data-stu-id="8eae3-107">Azure Data Lake Tools for Visual Studio provides a feature called **Failed Vertex Debug**, which lets you clone a failed job from hello cloud tooyour local machine for debugging.</span></span> <span data-ttu-id="8eae3-108">hello lokala klona avbildar hello hela molnmiljö, inklusive alla indata och användarkod.</span><span class="sxs-lookup"><span data-stu-id="8eae3-108">hello local clone captures hello entire cloud environment, including any input data and user code.</span></span>

<span data-ttu-id="8eae3-109">hello följande videoklipp visar misslyckades Vertex Debug i Azure Data Lake-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8eae3-109">hello following video demonstrates Failed Vertex Debug in Azure Data Lake Tools for Visual Studio.</span></span>

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>

> [!NOTE]
> <span data-ttu-id="8eae3-110">Visual Studio kräver hello följande två uppdateringar om de inte redan är installerade: [Microsoft Visual C++ 2015 Redistributable uppdatering 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) och [Universal C Runtime för Windows](https://www.microsoft.com/download/details.aspx?id=50410).</span><span class="sxs-lookup"><span data-stu-id="8eae3-110">Visual Studio requires hello following two updates, if they are not already installed: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) and the [Universal C Runtime for Windows](https://www.microsoft.com/download/details.aspx?id=50410).</span></span>

## <a name="download-failed-vertex-toolocal-machine"></a><span data-ttu-id="8eae3-111">Gick inte att hämta vertex toolocal datorn</span><span class="sxs-lookup"><span data-stu-id="8eae3-111">Download failed vertex toolocal machine</span></span>

<span data-ttu-id="8eae3-112">När du öppnar ett jobb som misslyckades i Azure Data Lake-verktyg för Visual Studio kan du se ett gult avisering fält med detaljerade felmeddelanden hello fel på fliken.</span><span class="sxs-lookup"><span data-stu-id="8eae3-112">When you open a failed job in Azure Data Lake Tools for Visual Studio, you see a yellow alert bar with detailed error messages in hello error tab.</span></span>

1. <span data-ttu-id="8eae3-113">Klicka på **hämta** toodownload alla hello nödvändiga resurser och inkommande dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="8eae3-113">Click **Download** toodownload all hello required resources and input streams.</span></span> <span data-ttu-id="8eae3-114">Om hello hämtningen inte slutförs, klicka på **försök**.</span><span class="sxs-lookup"><span data-stu-id="8eae3-114">If hello download doesn't complete, click **Retry**.</span></span>

2. <span data-ttu-id="8eae3-115">Klicka på **öppna** när hello nedladdningen är klar toogenerate lokala felsökningsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8eae3-115">Click **Open** after hello download completes toogenerate a local debugging environment.</span></span> <span data-ttu-id="8eae3-116">En ny Visual Studio-instans med en lösning för felsökning skapas och öppnas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="8eae3-116">A new Visual Studio instance with a debugging solution is automatically created and opened.</span></span>

![Azure Data Lake Analytics U-SQL felsökning i visual studio download vertex](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

<span data-ttu-id="8eae3-118">Jobb kan inkludera bakomliggande kod källfiler eller registrerade sammansättningar, och dessa två typer har olika scenarier för felsökning.</span><span class="sxs-lookup"><span data-stu-id="8eae3-118">Jobs may include code-behind source files or registered assemblies, and these two types have different debugging scenarios.</span></span>

- [<span data-ttu-id="8eae3-119">Felsöka ett jobb som misslyckades med bakomliggande kod</span><span class="sxs-lookup"><span data-stu-id="8eae3-119">Debug a failed job with code-behind</span></span>](#debug-job-failed-with-code-behind)
- [<span data-ttu-id="8eae3-120">Felsöka ett jobb som misslyckades med sammansättningar</span><span class="sxs-lookup"><span data-stu-id="8eae3-120">Debug a failed job with assemblies</span></span>](#debug-job-failed-with-assemblies)


## <a name="debug-job-failed-with-code-behind"></a><span data-ttu-id="8eae3-121">Felsöka jobb som misslyckades med bakomliggande kod</span><span class="sxs-lookup"><span data-stu-id="8eae3-121">Debug job failed with code-behind</span></span>

<span data-ttu-id="8eae3-122">Om ett U-SQL-jobb misslyckas och hello arbetsuppgifter inbegriper användarkod (vanligtvis namnet `Script.usql.cs` i ett U-SQL-projekt), att källkoden har importerats till hello felsökning lösning.</span><span class="sxs-lookup"><span data-stu-id="8eae3-122">If a U-SQL job fails, and hello job includes user code (typically named `Script.usql.cs` in a U-SQL project), that source code is imported into hello debugging solution.</span></span>  <span data-ttu-id="8eae3-123">Därifrån kan du använda hello Visual Studio felsökning verktyg (titta på, variabler, etc.) tootroubleshoot hello problem.</span><span class="sxs-lookup"><span data-stu-id="8eae3-123">From there you can use hello Visual Studio debugging tools (watch, variables, etc.) tootroubleshoot hello problem.</span></span>

> [!NOTE]
> <span data-ttu-id="8eae3-124">Innan du felsökning, vara säker på att toocheck **Common Language Runtime undantag** i hello undantag inställningar fönster (**Ctrl + Alt + E**).</span><span class="sxs-lookup"><span data-stu-id="8eae3-124">Before debugging, be sure toocheck **Common Language Runtime Exceptions** in hello Exception Settings window (**Ctrl + Alt + E**).</span></span>

![Azure Data Lake Analytics U-SQL felsökning i visual studio-inställning](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. <span data-ttu-id="8eae3-126">Tryck på **F5** toorun hello bakomliggande kod kod.</span><span class="sxs-lookup"><span data-stu-id="8eae3-126">Press **F5** toorun hello code-behind code.</span></span> <span data-ttu-id="8eae3-127">Det kan köras förrän den har stoppats av ett undantagsfel.</span><span class="sxs-lookup"><span data-stu-id="8eae3-127">It will run until it is stopped by an exception.</span></span>

2. <span data-ttu-id="8eae3-128">Öppna hello `ADLTool_Codebehind.usql.cs` filen och ange brytpunkter, tryck på **F5** toodebug hello kod steg för steg.</span><span class="sxs-lookup"><span data-stu-id="8eae3-128">Open hello `ADLTool_Codebehind.usql.cs` file and set breakpoints, then press **F5** toodebug hello code step by step.</span></span>

    ![Azure Data Lake Analytics U-SQL debug-undantag](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a><span data-ttu-id="8eae3-130">Felsöka jobb som misslyckades med sammansättningar</span><span class="sxs-lookup"><span data-stu-id="8eae3-130">Debug job failed with assemblies</span></span>

<span data-ttu-id="8eae3-131">Om du använder registrerade sammansättningar i U-SQL-skript kan hello system kan inte hämta hello källkoden automatiskt.</span><span class="sxs-lookup"><span data-stu-id="8eae3-131">If you use registered assemblies in your U-SQL script, hello system can't get hello source code automatically.</span></span> <span data-ttu-id="8eae3-132">I det här fallet manuellt lägga till hello sammansättningar källa kod filer toohello lösning.</span><span class="sxs-lookup"><span data-stu-id="8eae3-132">In this case, manually add hello assemblies' source code files toohello solution.</span></span>

### <a name="configure-hello-solution"></a><span data-ttu-id="8eae3-133">Konfigurera hello lösning</span><span class="sxs-lookup"><span data-stu-id="8eae3-133">Configure hello solution</span></span>

1. <span data-ttu-id="8eae3-134">Högerklicka på **lösning 'VertexDebug' > Lägg till > befintligt projekt...**  toofind hello sammansättningar källkoden och Lägg till hello projektet toohello felsökning lösning.</span><span class="sxs-lookup"><span data-stu-id="8eae3-134">Right-click **Solution 'VertexDebug' > Add > Existing Project...** toofind hello assemblies' source code and add hello project toohello debugging solution.</span></span>

    ![Azure Data Lake Analytics U-SQL-debug lägga till projekt](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. <span data-ttu-id="8eae3-136">Högerklicka på **LocalVertexHost > Egenskaper** i hello lösning och kopiera hello **Working Directory** sökväg.</span><span class="sxs-lookup"><span data-stu-id="8eae3-136">Right-click **LocalVertexHost > Properties** in hello solution and copy hello **Working Directory** path.</span></span>

3. <span data-ttu-id="8eae3-137">Högerklicka på **sammansättningsprojekt källa kod > Egenskaper**väljer hello **skapa** fliken längst till vänster och klistra in hello kopieras sökväg som **utdata > Utdatasökvägen**.</span><span class="sxs-lookup"><span data-stu-id="8eae3-137">Right-Click **assembly source code project > Properties**, select hello **Build** tab at left, and paste hello copied path as **Output > Output path**.</span></span>

    ![Ange sökväg till pdb för Azure Data Lake Analytics U-SQL-felsökning](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. <span data-ttu-id="8eae3-139">Tryck på **Ctrl + Alt + E**, kontrollera **Common Language Runtime undantag** i inställningar för undantag.</span><span class="sxs-lookup"><span data-stu-id="8eae3-139">Press **Ctrl + Alt + E**, check **Common Language Runtime Exceptions** in Exception Settings window.</span></span>

### <a name="start-debug"></a><span data-ttu-id="8eae3-140">Starta felsökning</span><span class="sxs-lookup"><span data-stu-id="8eae3-140">Start debug</span></span>

1. <span data-ttu-id="8eae3-141">Högerklicka på **Kodprojekt för sammansättningen källa > återskapa** toooutput .pdb filer toohello `LocalVertexHost` arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="8eae3-141">Right-click **assembly source code project > Rebuild** toooutput .pdb files toohello `LocalVertexHost` working directory.</span></span>

2. <span data-ttu-id="8eae3-142">Tryck på **F5** och hello projektet kommer att köras tills den har stoppats av ett undantagsfel.</span><span class="sxs-lookup"><span data-stu-id="8eae3-142">Press **F5** and hello project will run until it is stopped by an exception.</span></span> <span data-ttu-id="8eae3-143">Du kan se hello följande varningsmeddelande som du kan ignorera.</span><span class="sxs-lookup"><span data-stu-id="8eae3-143">You may see hello following warning message, which you can safely ignore.</span></span> <span data-ttu-id="8eae3-144">Det kan ta upp tooa minut tooget toohello debug skärmen.</span><span class="sxs-lookup"><span data-stu-id="8eae3-144">It can take up tooa minute tooget toohello debug screen.</span></span>

    ![Azure Data Lake Analytics U-SQL felsökning i visual studio-varning](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. <span data-ttu-id="8eae3-146">Öppna källkoden och ange brytpunkter, tryck på **F5** toodebug hello kod steg för steg.</span><span class="sxs-lookup"><span data-stu-id="8eae3-146">Open your source code and set breakpoints, then press **F5** toodebug hello code step by step.</span></span>

<span data-ttu-id="8eae3-147">Du kan också använda hello Visual Studio felsökning verktyg (titta på, variabler, etc.) tootroubleshoot hello problem.</span><span class="sxs-lookup"><span data-stu-id="8eae3-147">You can also use hello Visual Studio debugging tools (watch, variables, etc.) tootroubleshoot hello problem.</span></span>

> [!NOTE]
> <span data-ttu-id="8eae3-148">Återskapa hello sammansättningsprojekt källa kod varje gång när du har ändrat hello kodfiler toogenerate uppdateras .pdb.</span><span class="sxs-lookup"><span data-stu-id="8eae3-148">Rebuild hello assembly source code project each time after you modify hello code toogenerate updated .pdb files.</span></span>

<span data-ttu-id="8eae3-149">Efter felsökning visar hello utdatafönstret hello efter meddelande om hello projekt slutförs:</span><span class="sxs-lookup"><span data-stu-id="8eae3-149">After debugging, if hello project completes successfully hello output window shows hello following message:</span></span>

```
hello Program 'LocalVertexHost.exe' has exited with code 0 (0x0).
```

![Azure Data Lake Analytics U-SQL-debug lyckas](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-hello-job"></a><span data-ttu-id="8eae3-151">Skicka hello jobb</span><span class="sxs-lookup"><span data-stu-id="8eae3-151">Resubmit hello job</span></span>

<span data-ttu-id="8eae3-152">När du har slutfört felsökning, skicka hello misslyckade jobbet.</span><span class="sxs-lookup"><span data-stu-id="8eae3-152">Once you have completed debugging, resubmit hello failed job.</span></span>

1. <span data-ttu-id="8eae3-153">Kopiera C#-kod för jobb med bakomliggande kod lösningar till hello bakomliggande kod källfilen (vanligtvis `Script.usql.cs`).</span><span class="sxs-lookup"><span data-stu-id="8eae3-153">For jobs with code-behind solutions, copy your C# code into hello code-behind source file (typically `Script.usql.cs`).</span></span>
2. <span data-ttu-id="8eae3-154">Registrera hello uppdateras .dll sammansättningar i databasen ADLA för jobb med sammansättningar:</span><span class="sxs-lookup"><span data-stu-id="8eae3-154">For jobs with assemblies, register hello updated .dll assemblies into your ADLA database:</span></span>
    1. <span data-ttu-id="8eae3-155">Från Server Explorer eller Cloud Explorer expanderar du hello **ADLA konto > databaser** nod.</span><span class="sxs-lookup"><span data-stu-id="8eae3-155">From Server Explorer or Cloud Explorer, expand hello **ADLA account > Databases** node.</span></span>
    2. <span data-ttu-id="8eae3-156">Högerklicka på **sammansättningar** och registrera ditt nya .dll-sammansättningar med hello ADLA databasen: ![Azure Data Lake Analytics U-SQL-debug registrera sammansättningen](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)</span><span class="sxs-lookup"><span data-stu-id="8eae3-156">Right-click **Assemblies** and register your new .dll assemblies with hello ADLA database: ![Azure Data Lake Analytics U-SQL debug register assembly](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)</span></span>
3. <span data-ttu-id="8eae3-157">Skicka jobbet.</span><span class="sxs-lookup"><span data-stu-id="8eae3-157">Resubmit your job.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8eae3-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8eae3-158">Next steps</span></span>

- [<span data-ttu-id="8eae3-159">U-SQL-programmering guide</span><span class="sxs-lookup"><span data-stu-id="8eae3-159">U-SQL programmability guide</span></span>](data-lake-analytics-u-sql-programmability-guide.md)
- [<span data-ttu-id="8eae3-160">Utveckla U-SQL-användardefinierade operatorer för Azure Data Lake Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="8eae3-160">Develop U-SQL User-defined operators for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [<span data-ttu-id="8eae3-161">Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8eae3-161">Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
