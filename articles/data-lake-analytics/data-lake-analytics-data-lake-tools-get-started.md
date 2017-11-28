---
title: "aaaDevelop U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio | Microsoft Docs"
description: "Lär dig hur tooinstall Data Lake-verktyg för Visual Studio och hur toodevelop och testa U-SQL-skript."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/28/2017
ms.author: saveenr, yanacai
ms.openlocfilehash: 7a0c02c275b8cefecbe784ba63969cbf24a150d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a><span data-ttu-id="68837-103">Utveckla U-SQL-skript med hjälp av Data Lake Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68837-103">Develop U-SQL scripts by using Data Lake Tools for Visual Studio</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


<span data-ttu-id="68837-104">Lär dig hur toouse Visual Studio toocreate Azure Data Lake Analytics-konton, definiera jobb i [U-SQL](data-lake-analytics-u-sql-get-started.md), och skicka jobb toohello Data Lake Analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="68837-104">Learn how toouse Visual Studio toocreate Azure Data Lake Analytics accounts, define jobs in [U-SQL](data-lake-analytics-u-sql-get-started.md), and submit jobs toohello Data Lake Analytics service.</span></span> <span data-ttu-id="68837-105">Mer information om Data Lake Analytics finns i [Översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="68837-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="68837-106">Krav</span><span class="sxs-lookup"><span data-stu-id="68837-106">Prerequisites</span></span>

* <span data-ttu-id="68837-107">**Visual Studio**: Alla utgåvor utom Express stöds.</span><span class="sxs-lookup"><span data-stu-id="68837-107">**Visual Studio**: All editions except Express are supported.</span></span>
    * <span data-ttu-id="68837-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="68837-108">Visual Studio 2017</span></span>
    * <span data-ttu-id="68837-109">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="68837-109">Visual Studio 2015</span></span>
    * <span data-ttu-id="68837-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="68837-110">Visual Studio 2013</span></span>
* <span data-ttu-id="68837-111">**Microsoft Azure SDK för .NET** version 2.7.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="68837-111">**Microsoft Azure SDK for .NET** version 2.7.1 or later.</span></span>  <span data-ttu-id="68837-112">Installera den med hjälp av hello [installationsprogram för webbplattform](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="68837-112">Install it by using hello [Web platform installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="68837-113">Ett **Data Lake Analytics**-konto.</span><span class="sxs-lookup"><span data-stu-id="68837-113">A **Data Lake Analytics** account.</span></span> <span data-ttu-id="68837-114">toocreate ett konto, se [Kom igång med Azure Data Lake Analytics med hjälp av Azure portal](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="68837-114">toocreate an account, see [Get Started with Azure Data Lake Analytics using Azure portal](data-lake-analytics-get-started-portal.md).</span></span>

## <a name="install-azure-data-lake-tools-for-visual-studio"></a><span data-ttu-id="68837-115">Installera Azure Data Lake Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68837-115">Install Azure Data Lake Tools for Visual Studio</span></span> 

<span data-ttu-id="68837-116">Hämta och installera Azure Data Lake-verktyg för Visual Studio [från hello Download Center](http://aka.ms/adltoolsvs).</span><span class="sxs-lookup"><span data-stu-id="68837-116">Download and install Azure Data Lake Tools for Visual Studio [from hello Download Center](http://aka.ms/adltoolsvs).</span></span> <span data-ttu-id="68837-117">Efter installationen kontrollerar du att:</span><span class="sxs-lookup"><span data-stu-id="68837-117">After installation, note that:</span></span>
* <span data-ttu-id="68837-118">Hej **Server Explorer** > **Azure** noden innehåller en **Datasjöanalys** nod.</span><span class="sxs-lookup"><span data-stu-id="68837-118">hello **Server Explorer** > **Azure** node contains a **Data Lake Analytics** node.</span></span> 
* <span data-ttu-id="68837-119">Hej **verktyg** menyn har en **Data Lake** objekt.</span><span class="sxs-lookup"><span data-stu-id="68837-119">hello **Tools** menu has a **Data Lake** item.</span></span>

## <a name="connect-tooan-azure-data-lake-analytics-account"></a><span data-ttu-id="68837-120">Ansluta tooan Azure Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="68837-120">Connect tooan Azure Data Lake Analytics account</span></span>

1. <span data-ttu-id="68837-121">Öppna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="68837-121">Open Visual Studio.</span></span>
2. <span data-ttu-id="68837-122">Öppna Server Explorer genom att välja **Visa** > **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="68837-122">Open Server Explorer by selecting **View** > **Server Explorer**.</span></span>
3. <span data-ttu-id="68837-123">Högerklicka på **Azure**.</span><span class="sxs-lookup"><span data-stu-id="68837-123">Right-click **Azure**.</span></span> <span data-ttu-id="68837-124">Välj sedan **ansluta tooMicrosoft Azure-prenumeration** och följer instruktionerna för hello.</span><span class="sxs-lookup"><span data-stu-id="68837-124">Then select **Connect tooMicrosoft Azure Subscription** and follow hello instructions.</span></span>
4. <span data-ttu-id="68837-125">Välj **Azure** > **Data Lake Analytics** i Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="68837-125">In Server Explorer, select **Azure** > **Data Lake Analytics**.</span></span> <span data-ttu-id="68837-126">En lista över dina Data Lake Analytics-konton visas.</span><span class="sxs-lookup"><span data-stu-id="68837-126">You see a list of your Data Lake Analytics accounts.</span></span>


## <a name="write-your-first-u-sql-script"></a><span data-ttu-id="68837-127">Skriv ditt första U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="68837-127">Write your first U-SQL script</span></span>

<span data-ttu-id="68837-128">hello efter texten är ett enkelt U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="68837-128">hello following text is a simple U-SQL script.</span></span> <span data-ttu-id="68837-129">Den definierar en liten datamängd och skrivningar som dataset toohello standard Data Lake Store som en fil som kallas `/data.csv`.</span><span class="sxs-lookup"><span data-stu-id="68837-129">It defines a small dataset and writes that dataset toohello default Data Lake Store as a file called `/data.csv`.</span></span>

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

### <a name="submit-a-data-lake-analytics-job"></a><span data-ttu-id="68837-130">Skicka ett Data Lake Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="68837-130">Submit a Data Lake Analytics job</span></span>

1. <span data-ttu-id="68837-131">Välj **Arkiv** > **Nytt** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="68837-131">Select **File** > **New** > **Project**.</span></span>

2. <span data-ttu-id="68837-132">Välj hello **U-SQL-projekt** Skriv och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="68837-132">Select hello **U-SQL Project** type, and then click **OK**.</span></span> <span data-ttu-id="68837-133">Visual Studio skapar en lösning med filen **Script.usql**.</span><span class="sxs-lookup"><span data-stu-id="68837-133">Visual Studio creates a solution with a **Script.usql** file.</span></span>

3. <span data-ttu-id="68837-134">Klistra in hello föregående skript i hello **Script.usql** fönster.</span><span class="sxs-lookup"><span data-stu-id="68837-134">Paste hello previous script into hello **Script.usql** window.</span></span>

4. <span data-ttu-id="68837-135">I hello övre vänstra hörnet i hello **Script.usql** fönstret Ange hello Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="68837-135">In hello upper-left corner of hello **Script.usql** window, specify hello Data Lake Analytics account.</span></span>

    ![Skicka U-SQL Visual Studio-projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. <span data-ttu-id="68837-137">I hello övre vänstra hörnet i hello **Script.usql** väljer **skicka**.</span><span class="sxs-lookup"><span data-stu-id="68837-137">In hello upper-left corner of hello **Script.usql** window, select **Submit**.</span></span>
6. <span data-ttu-id="68837-138">Kontrollera hello **Analytics-konto**, och välj sedan **skicka**.</span><span class="sxs-lookup"><span data-stu-id="68837-138">Verify hello **Analytics Account**, and then select **Submit**.</span></span> <span data-ttu-id="68837-139">Resultat för skicka är tillgängliga i hello Data Lake-verktyg för Visual Studio-resultat när hello ansökan har slutförts.</span><span class="sxs-lookup"><span data-stu-id="68837-139">Submission results are available in hello Data Lake Tools for Visual Studio Results after hello submission is complete.</span></span>

    ![Skicka U-SQL Visual Studio-projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
7. <span data-ttu-id="68837-141">toosee hello senaste jobb status och uppdatera hello-skärmen, klickar du på **uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="68837-141">toosee hello latest job status and refresh hello screen, click **Refresh**.</span></span> <span data-ttu-id="68837-142">När hello jobb lyckas, den visar hello **Jobbdiagram**, **metadataåtgärder**, **Tillståndshistorik**, och **diagnostik**:</span><span class="sxs-lookup"><span data-stu-id="68837-142">When hello job succeeds, it shows hello **Job Graph**, **MetaData Operations**, **State History**, and **Diagnostics**:</span></span>

    ![Prestandadiagram för U-SQL Visual Studio Data Lake Analytics-jobbet](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * <span data-ttu-id="68837-144">**Jobbet sammanfattning** visar hello sammanfattning av hello jobb.</span><span class="sxs-lookup"><span data-stu-id="68837-144">**Job Summary** shows hello summary of hello job.</span></span>   
   * <span data-ttu-id="68837-145">**Jobbinformation** visar mer detaljerad information om hello jobbet, inklusive hello skript, resurser och formhörnen.</span><span class="sxs-lookup"><span data-stu-id="68837-145">**Job Details** shows more specific information about hello job, including hello script, resources, and vertices.</span></span>
   * <span data-ttu-id="68837-146">**Jobbdiagram** visualizes hello förloppet för jobbet hello.</span><span class="sxs-lookup"><span data-stu-id="68837-146">**Job Graph** visualizes hello progress of hello job.</span></span>
   * <span data-ttu-id="68837-147">**Metadataåtgärder** visar alla hello-åtgärder som vidtagits hello U-SQL-katalogen.</span><span class="sxs-lookup"><span data-stu-id="68837-147">**MetaData Operations** shows all hello actions that were taken on hello U-SQL catalog.</span></span>
   * <span data-ttu-id="68837-148">**Data** visar alla hello indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="68837-148">**Data** shows all hello inputs and outputs.</span></span>
   * <span data-ttu-id="68837-149">**Diagnostik** ger en avancerad analys för jobbkörning och prestandaoptimering.</span><span class="sxs-lookup"><span data-stu-id="68837-149">**Diagnostics** provides an advanced analysis for job execution and performance optimization.</span></span>

### <a name="toocheck-job-state"></a><span data-ttu-id="68837-150">toocheck jobbets status</span><span class="sxs-lookup"><span data-stu-id="68837-150">toocheck job state</span></span>

1. <span data-ttu-id="68837-151">Välj **Azure** > **Data Lake Analytics** i Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="68837-151">In Server Explorer, select **Azure** > **Data Lake Analytics**.</span></span> 
2. <span data-ttu-id="68837-152">Expandera hello Data Lake Analytics-kontonamnet.</span><span class="sxs-lookup"><span data-stu-id="68837-152">Expand hello Data Lake Analytics account name.</span></span>
3. <span data-ttu-id="68837-153">Dubbelklicka på **Jobb**.</span><span class="sxs-lookup"><span data-stu-id="68837-153">Double-click **Jobs**.</span></span>
4. <span data-ttu-id="68837-154">Välj hello jobb som du redan har skickat.</span><span class="sxs-lookup"><span data-stu-id="68837-154">Select hello job that you previously submitted.</span></span>

### <a name="toosee-hello-output-of-a-job"></a><span data-ttu-id="68837-155">toosee hello utdata för ett jobb</span><span class="sxs-lookup"><span data-stu-id="68837-155">toosee hello output of a job</span></span>

1. <span data-ttu-id="68837-156">Bläddra i Server Explorer toohello jobbet du skickat.</span><span class="sxs-lookup"><span data-stu-id="68837-156">In Server Explorer, browse toohello job you submitted.</span></span>
2. <span data-ttu-id="68837-157">Klicka på hello **Data** fliken.</span><span class="sxs-lookup"><span data-stu-id="68837-157">Click hello **Data** tab.</span></span>
3. <span data-ttu-id="68837-158">I hello **utdata för jobbet** fliken, väljer hello `"/data.csv"` fil.</span><span class="sxs-lookup"><span data-stu-id="68837-158">In hello **Job Outputs** tab, select hello `"/data.csv"` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="68837-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68837-159">Next steps</span></span>

* [<span data-ttu-id="68837-160">Kör U-SQL-skript på din egen arbetsstation för testning och felsökning</span><span class="sxs-lookup"><span data-stu-id="68837-160">Run U-SQL scripts on your own workstation for testing and debugging</span></span>](data-lake-analytics-data-lake-tools-local-run.md)
* [<span data-ttu-id="68837-161">Felsöka C#-kod i U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="68837-161">Debug C# code in U-SQL jobs</span></span>](data-lake-analytics-debug-u-sql-jobs.md)
* [<span data-ttu-id="68837-162">Använd hello Azure Data Lake-verktyg för Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="68837-162">Use hello Azure Data Lake Tools for Visual Studio Code</span></span>](data-lake-analytics-data-lake-tools-for-vscode.md)
