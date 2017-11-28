---
title: "aaaAnalyze webbplatsloggar med hjälp av Azure Data Lake Analytics | Microsoft Docs"
description: "Lär dig hur tooanalyze webbplatsloggar med hjälp av Data Lake Analytics. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 3a196735-d0d9-4deb-ba68-c4b3f3be8403
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: d27aaca95ed2b643cfed7a17b0066bf7fa4a1bf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-website-logs-using-azure-data-lake-analytics"></a><span data-ttu-id="c94bd-103">Analysera webbplatsloggar med hjälp av Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="c94bd-103">Analyze Website logs using Azure Data Lake Analytics</span></span>
<span data-ttu-id="c94bd-104">Lär dig hur tooanalyze webbplatsloggar med hjälp av Data Lake Analytics, särskilt för att ta reda på vilka referenter råkade ut för fel när de försöker toovisit hello webbplats.</span><span class="sxs-lookup"><span data-stu-id="c94bd-104">Learn how tooanalyze website logs using Data Lake Analytics, especially on finding out which referrers ran into errors when they tried toovisit hello website.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c94bd-105">Krav</span><span class="sxs-lookup"><span data-stu-id="c94bd-105">Prerequisites</span></span>
* <span data-ttu-id="c94bd-106">**Visual Studio 2015 eller Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-106">**Visual Studio 2015 or Visual Studio 2013**.</span></span>
* <span data-ttu-id="c94bd-107">**[Data Lake-verktyg för Visual Studio](http://aka.ms/adltoolsvs)**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-107">**[Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs)**.</span></span>

    <span data-ttu-id="c94bd-108">När Data Lake-verktyg för Visual Studio är installerat visas ett **Datasjö** objekt i hello **verktyg** -menyn i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c94bd-108">Once Data Lake Tools for Visual Studio is installed, you will see a **Data Lake** item in hello **Tools** menu in Visual Studio:</span></span>

    ![U-SQL Visual Studio-menyn](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)
* <span data-ttu-id="c94bd-110">**Grundläggande kunskaper om Data Lake Analytics och hello Data Lake-verktyg för Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-110">**Basic knowledge of Data Lake Analytics and hello Data Lake Tools for Visual Studio**.</span></span> <span data-ttu-id="c94bd-111">tooget igång, se:</span><span class="sxs-lookup"><span data-stu-id="c94bd-111">tooget started, see:</span></span>

  * <span data-ttu-id="c94bd-112">[Utveckla U-SQL-skript med Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c94bd-112">[Develop U-SQL script using Data Lake tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="c94bd-113">**Ett Data Lake Analytics-konto.**</span><span class="sxs-lookup"><span data-stu-id="c94bd-113">**A Data Lake Analytics account.**</span></span>  <span data-ttu-id="c94bd-114">Se [skapa ett Azure Data Lake Analytics-konto](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c94bd-114">See [Create an Azure Data Lake Analytics account](data-lake-analytics-get-started-portal.md).</span></span>
* <span data-ttu-id="c94bd-115">**Överför hello exempel data toohello Data Lake Analytics-konto.**</span><span class="sxs-lookup"><span data-stu-id="c94bd-115">**Upload hello sample data toohello Data Lake Analytics account.**</span></span> <span data-ttu-id="c94bd-116">Se [toocopy exempeldatafiler](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c94bd-116">See [toocopy sample data files](data-lake-analytics-get-started-portal.md).</span></span>

    <span data-ttu-id="c94bd-117">toorun ett Data Lake Analytics-jobb, måste vissa data.</span><span class="sxs-lookup"><span data-stu-id="c94bd-117">toorun a Data Lake Analytics job, you will need some data.</span></span> <span data-ttu-id="c94bd-118">Även om hello Data Lake-verktygen stöder uppladdning av data, använder du hello portal tooupload hello exempel data toomake den här självstudiekursen enklare toofollow.</span><span class="sxs-lookup"><span data-stu-id="c94bd-118">Even though hello Data Lake Tools supports uploading data, you will use hello portal tooupload hello sample data toomake this tutorial easier toofollow.</span></span>

## <a name="connect-tooazure"></a><span data-ttu-id="c94bd-119">Ansluta tooAzure</span><span class="sxs-lookup"><span data-stu-id="c94bd-119">Connect tooAzure</span></span>
<span data-ttu-id="c94bd-120">Innan du kan skapa och testa alla U-SQL-skript, måste du först ansluta tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c94bd-120">Before you can build and test any U-SQL scripts, you must first connect tooAzure.</span></span>

<span data-ttu-id="c94bd-121">**tooconnect tooData Lake Analytics**</span><span class="sxs-lookup"><span data-stu-id="c94bd-121">**tooconnect tooData Lake Analytics**</span></span>

1. <span data-ttu-id="c94bd-122">Öppna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c94bd-122">Open Visual Studio.</span></span>
2. <span data-ttu-id="c94bd-123">Klicka på **Data Lake > Alternativ och inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-123">Click **Data Lake > Options and Settings**.</span></span>
3. <span data-ttu-id="c94bd-124">Klicka på **logga In**, eller **ändra användare** om någon har loggat in och följ instruktionerna för hello.</span><span class="sxs-lookup"><span data-stu-id="c94bd-124">Click **Sign In**, or **Change User** if someone has signed in, and follow hello instructions.</span></span>
4. <span data-ttu-id="c94bd-125">Klicka på **OK** tooclose hello alternativ och inställningar för dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c94bd-125">Click **OK** tooclose hello Options and Settings dialog.</span></span>

<span data-ttu-id="c94bd-126">**toobrowse Data Lake Analytics-konton**</span><span class="sxs-lookup"><span data-stu-id="c94bd-126">**toobrowse your Data Lake Analytics accounts**</span></span>

1. <span data-ttu-id="c94bd-127">Visual Studio, öppna **Server Explorer** genom att klicka på **CTRL + ALT + S**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-127">From Visual Studio, open **Server Explorer** by press **CTRL+ALT+S**.</span></span>
2. <span data-ttu-id="c94bd-128">Gå till **Server Explorer**, expandera **Azure** och expandera sedan **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-128">From **Server Explorer**, expand **Azure**, and then expand **Data Lake Analytics**.</span></span> <span data-ttu-id="c94bd-129">En lista över dina Data Lake Analytics-konton visas om det finns några.</span><span class="sxs-lookup"><span data-stu-id="c94bd-129">You shall see a list of your Data Lake Analytics accounts if there are any.</span></span> <span data-ttu-id="c94bd-130">Du kan inte skapa Data Lake Analytics-konton från hello studio.</span><span class="sxs-lookup"><span data-stu-id="c94bd-130">You cannot create Data Lake Analytics accounts from hello studio.</span></span> <span data-ttu-id="c94bd-131">toocreate ett konto, se [Kom igång med Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-get-started-portal.md) eller [Kom igång med Azure Data Lake Analytics med hjälp av Azure PowerShell](data-lake-analytics-get-started-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="c94bd-131">toocreate an account, see [Get Started with Azure Data Lake Analytics using Azure Portal](data-lake-analytics-get-started-portal.md) or [Get Started with Azure Data Lake Analytics using Azure PowerShell](data-lake-analytics-get-started-powershell.md).</span></span>

## <a name="develop-u-sql-application"></a><span data-ttu-id="c94bd-132">Utveckla U-SQL-program</span><span class="sxs-lookup"><span data-stu-id="c94bd-132">Develop U-SQL application</span></span>
<span data-ttu-id="c94bd-133">Ett U-SQL-program är oftast ett U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="c94bd-133">A U-SQL application is mostly a U-SQL script.</span></span> <span data-ttu-id="c94bd-134">toolearn mer om U-SQL finns [Kom igång med U-SQL](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c94bd-134">toolearn more about U-SQL, see [Get started with U-SQL](data-lake-analytics-u-sql-get-started.md).</span></span>

<span data-ttu-id="c94bd-135">Du kan lägga till ytterligare användardefinierade operatorer toohello program.</span><span class="sxs-lookup"><span data-stu-id="c94bd-135">You can add addition user-defined operators toohello application.</span></span>  <span data-ttu-id="c94bd-136">Mer information finns i [utveckla U-SQL-operatörer för Data Lake Analytics-jobb för användardefinierade](data-lake-analytics-u-sql-develop-user-defined-operators.md).</span><span class="sxs-lookup"><span data-stu-id="c94bd-136">For more information, see [Develop U-SQL user defined operators for Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-user-defined-operators.md).</span></span>

<span data-ttu-id="c94bd-137">**toocreate och skicka ett Data Lake Analytics-jobb**</span><span class="sxs-lookup"><span data-stu-id="c94bd-137">**toocreate and submit a Data Lake Analytics job**</span></span>

1. <span data-ttu-id="c94bd-138">Klicka på hello **Arkiv > Nytt > projekt**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-138">Click hello **File > New > Project**.</span></span>
2. <span data-ttu-id="c94bd-139">Ange hello U-SQL-projekt.</span><span class="sxs-lookup"><span data-stu-id="c94bd-139">Select hello U-SQL Project type.</span></span>

    ![nytt U-SQL Visual Studio-projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. <span data-ttu-id="c94bd-141">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-141">Click **OK**.</span></span> <span data-ttu-id="c94bd-142">Visual studio skapar en lösning med en Script.usql-fil.</span><span class="sxs-lookup"><span data-stu-id="c94bd-142">Visual studio creates a solution with a Script.usql file.</span></span>
4. <span data-ttu-id="c94bd-143">Ange följande skript i hello Script.usql fil hello:</span><span class="sxs-lookup"><span data-stu-id="c94bd-143">Enter hello following script into hello Script.usql file:</span></span>

        // Create a database for easy reuse, so you don't need tooread from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from hello weblog file with hello correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            DISTRIBUTED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    <span data-ttu-id="c94bd-144">toounderstand hello U-SQL finns [Kom igång med Data Lake Analytics U-SQL-språket](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c94bd-144">toounderstand hello U-SQL, see [Get started with Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>    
5. <span data-ttu-id="c94bd-145">Lägg till ett nytt U-SQL-skript tooyour projekt och ange hello följande:</span><span class="sxs-lookup"><span data-stu-id="c94bd-145">Add a new U-SQL script tooyour project and enter hello following:</span></span>

        // Query hello referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        too@"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();
6. <span data-ttu-id="c94bd-146">Växla tillbaka toohello första U-SQL-skript och nästa toohello **skicka** knappen, ange ditt Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="c94bd-146">Switch back toohello first U-SQL script and next toohello **Submit** button, specify your Analytics account.</span></span>
7. <span data-ttu-id="c94bd-147">Från **Solution Explorer**, högerklicka på **Script.usql** och klicka sedan på **Skapa skript**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-147">From **Solution Explorer**, right click **Script.usql**, and then click **Build Script**.</span></span> <span data-ttu-id="c94bd-148">Kontrollera hello resulterar i hello utdatafönstret.</span><span class="sxs-lookup"><span data-stu-id="c94bd-148">Verify hello results in hello Output pane.</span></span>
8. <span data-ttu-id="c94bd-149">Från **Solution Explorer**, högerklicka på **Script.usql** och klicka sedan på **Skicka skript**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-149">From **Solution Explorer**, right click **Script.usql**, and then click **Submit Script**.</span></span>
9. <span data-ttu-id="c94bd-150">Kontrollera hello **Analytics-konto** är hello en där du vill toorun hello jobbet och klicka sedan på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-150">Verify hello **Analytics Account** is hello one where you want toorun hello job, and then click **Submit**.</span></span> <span data-ttu-id="c94bd-151">Resultat för skicka och jobblänk är tillgängliga i hello Data Lake-verktyg för Visual Studio resultatfönstret när hello överföringen är klar.</span><span class="sxs-lookup"><span data-stu-id="c94bd-151">Submission results and job link are available in hello Data Lake Tools for Visual Studio Results window when hello submission is completed.</span></span>
10. <span data-ttu-id="c94bd-152">Vänta tills hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="c94bd-152">Wait until hello job is completed successfully.</span></span>  <span data-ttu-id="c94bd-153">Om hello misslyckades, saknar troligen hello källfilen.</span><span class="sxs-lookup"><span data-stu-id="c94bd-153">If hello job failed, it is most likely missing hello source file.</span></span>  <span data-ttu-id="c94bd-154">Se hello nödvändiga avsnitt i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="c94bd-154">Please see hello Prerequisite section of this tutorial.</span></span> <span data-ttu-id="c94bd-155">Ytterligare felsökningsinformation finns i [övervaka och felsöka Azure Data Lake Analytics-jobb](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="c94bd-155">For additional troubleshooting information, see [Monitor and troubleshoot Azure Data Lake Analytics jobs](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span></span>

    <span data-ttu-id="c94bd-156">När hello jobbet har slutförts, skall hello följande skärm visas:</span><span class="sxs-lookup"><span data-stu-id="c94bd-156">When hello job is completed, you shall see hello following screen:</span></span>

    ![datasjöanalys analysera webbloggar webbplatsloggar](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)
11. <span data-ttu-id="c94bd-158">Upprepa steg 7 – 10 för **Script1.usql**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-158">Now repeat steps 7- 10 for **Script1.usql**.</span></span>

<span data-ttu-id="c94bd-159">**toosee hello jobbutdata**</span><span class="sxs-lookup"><span data-stu-id="c94bd-159">**toosee hello job output**</span></span>

1. <span data-ttu-id="c94bd-160">Från **Server Explorer**, expandera **Azure**, expandera **Datasjöanalys**, expandera Data Lake Analytics-kontot, expandera **Lagringskonton**, högerklicka på hello Data Lake-standardlagringskontot och klicka sedan på **Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-160">From **Server Explorer**, expand **Azure**, expand **Data Lake Analytics**, expand your Data Lake Analytics account, expand **Storage Accounts**, right-click hello default Data Lake Storage account, and then click **Explorer**.</span></span>
2. <span data-ttu-id="c94bd-161">Dubbelklicka på **exempel** tooopen hello mapp och sedan dubbelklicka på **utdata**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-161">Double-click **Samples** tooopen hello folder, and then double-click **Outputs**.</span></span>
3. <span data-ttu-id="c94bd-162">Dubbelklicka på **UnsuccessfulResponsees.log**.</span><span class="sxs-lookup"><span data-stu-id="c94bd-162">Double-click **UnsuccessfulResponsees.log**.</span></span>
4. <span data-ttu-id="c94bd-163">Du kan också dubbelklicka hello utdatafilen inuti hello diagramvyn för hello jobbet i ordning toonavigate direkt toohello utdata.</span><span class="sxs-lookup"><span data-stu-id="c94bd-163">You can also double-click hello output file inside hello graph view of hello job in order toonavigate directly toohello output.</span></span>

## <a name="see-also"></a><span data-ttu-id="c94bd-164">Se även</span><span class="sxs-lookup"><span data-stu-id="c94bd-164">See also</span></span>
<span data-ttu-id="c94bd-165">tooget igång med Data Lake Analytics med hjälp av olika verktyg, se:</span><span class="sxs-lookup"><span data-stu-id="c94bd-165">tooget started with Data Lake Analytics using different tools, see:</span></span>

* [<span data-ttu-id="c94bd-166">Kom igång med Data Lake Analytics med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c94bd-166">Get started with Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="c94bd-167">Kom igång med Data Lake Analytics med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c94bd-167">Get started with Data Lake Analytics using Azure PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="c94bd-168">Kom igång med Data Lake Analytics med hjälp av NET SDK</span><span class="sxs-lookup"><span data-stu-id="c94bd-168">Get started with Data Lake Analytics using .NET SDK</span></span>](data-lake-analytics-get-started-net-sdk.md)
