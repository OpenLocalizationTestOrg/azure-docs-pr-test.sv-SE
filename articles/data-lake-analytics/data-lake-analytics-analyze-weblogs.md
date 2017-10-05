---
title: "Analysera webbplatsloggar med hjälp av Azure Data Lake Analytics | Microsoft Docs"
description: "Lär dig att analysera webbplatsloggar med hjälp av Data Lake Analytics. "
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
ms.openlocfilehash: 25fbbe97d26491fc421f4821315761c18e523ec8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-website-logs-using-azure-data-lake-analytics"></a><span data-ttu-id="67905-103">Analysera webbplatsloggar med hjälp av Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="67905-103">Analyze Website logs using Azure Data Lake Analytics</span></span>
<span data-ttu-id="67905-104">Lär dig att analysera webbplatsloggar med hjälp av Data Lake Analytics, särskilt för att ta reda på vilka referenter råkade ut för fel när de försöker att gå till webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="67905-104">Learn how to analyze website logs using Data Lake Analytics, especially on finding out which referrers ran into errors when they tried to visit the website.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67905-105">Krav</span><span class="sxs-lookup"><span data-stu-id="67905-105">Prerequisites</span></span>
* <span data-ttu-id="67905-106">**Visual Studio 2015 eller Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="67905-106">**Visual Studio 2015 or Visual Studio 2013**.</span></span>
* <span data-ttu-id="67905-107">**[Data Lake-verktyg för Visual Studio](http://aka.ms/adltoolsvs)**.</span><span class="sxs-lookup"><span data-stu-id="67905-107">**[Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs)**.</span></span>

    <span data-ttu-id="67905-108">När Data Lake-verktyg för Visual Studio är installerat visas en **Datasjö** objektet i den **verktyg** -menyn i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="67905-108">Once Data Lake Tools for Visual Studio is installed, you will see a **Data Lake** item in the **Tools** menu in Visual Studio:</span></span>

    ![U-SQL Visual Studio-menyn](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)
* <span data-ttu-id="67905-110">**Grundläggande kunskaper om Data Lake Analytics och Data Lake-verktyg för Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="67905-110">**Basic knowledge of Data Lake Analytics and the Data Lake Tools for Visual Studio**.</span></span> <span data-ttu-id="67905-111">Om du vill komma igång, se:</span><span class="sxs-lookup"><span data-stu-id="67905-111">To get started, see:</span></span>

  * <span data-ttu-id="67905-112">[Utveckla U-SQL-skript med Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="67905-112">[Develop U-SQL script using Data Lake tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="67905-113">**Ett Data Lake Analytics-konto.**</span><span class="sxs-lookup"><span data-stu-id="67905-113">**A Data Lake Analytics account.**</span></span>  <span data-ttu-id="67905-114">Se [skapa ett Azure Data Lake Analytics-konto](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="67905-114">See [Create an Azure Data Lake Analytics account](data-lake-analytics-get-started-portal.md).</span></span>
* <span data-ttu-id="67905-115">**Ladda upp exempeldata till Data Lake Analytics-konto.**</span><span class="sxs-lookup"><span data-stu-id="67905-115">**Upload the sample data to the Data Lake Analytics account.**</span></span> <span data-ttu-id="67905-116">Se [att kopiera exempeldatafilerna](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="67905-116">See [To copy sample data files](data-lake-analytics-get-started-portal.md).</span></span>

    <span data-ttu-id="67905-117">Om du vill köra ett Data Lake Analytics-jobb behöver du vissa data.</span><span class="sxs-lookup"><span data-stu-id="67905-117">To run a Data Lake Analytics job, you will need some data.</span></span> <span data-ttu-id="67905-118">Även om Data Lake-verktygen stöder uppladdning av data, kommer du att använda portalen för att ladda upp exempeldata. Detta gör det lättare att följa dessa självstudier.</span><span class="sxs-lookup"><span data-stu-id="67905-118">Even though the Data Lake Tools supports uploading data, you will use the portal to upload the sample data to make this tutorial easier to follow.</span></span>

## <a name="connect-to-azure"></a><span data-ttu-id="67905-119">Anslut till Azure</span><span class="sxs-lookup"><span data-stu-id="67905-119">Connect to Azure</span></span>
<span data-ttu-id="67905-120">Innan du kan skapa och testa alla U-SQL-skript, måste du först ansluta till Azure.</span><span class="sxs-lookup"><span data-stu-id="67905-120">Before you can build and test any U-SQL scripts, you must first connect to Azure.</span></span>

<span data-ttu-id="67905-121">**Så här ansluter du till Data Lake Analytics**</span><span class="sxs-lookup"><span data-stu-id="67905-121">**To connect to Data Lake Analytics**</span></span>

1. <span data-ttu-id="67905-122">Öppna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="67905-122">Open Visual Studio.</span></span>
2. <span data-ttu-id="67905-123">Klicka på **Data Lake > Alternativ och inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="67905-123">Click **Data Lake > Options and Settings**.</span></span>
3. <span data-ttu-id="67905-124">Klicka på **logga In**, eller **ändra användare** om någon har loggat in och följ instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="67905-124">Click **Sign In**, or **Change User** if someone has signed in, and follow the instructions.</span></span>
4. <span data-ttu-id="67905-125">Klicka på **OK** att stänga dialogrutan Alternativ och inställningar.</span><span class="sxs-lookup"><span data-stu-id="67905-125">Click **OK** to close the Options and Settings dialog.</span></span>

<span data-ttu-id="67905-126">**Att bläddra i Data Lake Analytics-konton**</span><span class="sxs-lookup"><span data-stu-id="67905-126">**To browse your Data Lake Analytics accounts**</span></span>

1. <span data-ttu-id="67905-127">Visual Studio, öppna **Server Explorer** genom att klicka på **CTRL + ALT + S**.</span><span class="sxs-lookup"><span data-stu-id="67905-127">From Visual Studio, open **Server Explorer** by press **CTRL+ALT+S**.</span></span>
2. <span data-ttu-id="67905-128">Gå till **Server Explorer**, expandera **Azure** och expandera sedan **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="67905-128">From **Server Explorer**, expand **Azure**, and then expand **Data Lake Analytics**.</span></span> <span data-ttu-id="67905-129">En lista över dina Data Lake Analytics-konton visas om det finns några.</span><span class="sxs-lookup"><span data-stu-id="67905-129">You shall see a list of your Data Lake Analytics accounts if there are any.</span></span> <span data-ttu-id="67905-130">Du kan inte skapa Data Lake Analytics-konton från studio.</span><span class="sxs-lookup"><span data-stu-id="67905-130">You cannot create Data Lake Analytics accounts from the studio.</span></span> <span data-ttu-id="67905-131">För att skapa ett konto, se [Kom igång med Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-get-started-portal.md) eller [Kom igång med Azure Data Lake Analytics med hjälp av Azure PowerShell](data-lake-analytics-get-started-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="67905-131">To create an account, see [Get Started with Azure Data Lake Analytics using Azure Portal](data-lake-analytics-get-started-portal.md) or [Get Started with Azure Data Lake Analytics using Azure PowerShell](data-lake-analytics-get-started-powershell.md).</span></span>

## <a name="develop-u-sql-application"></a><span data-ttu-id="67905-132">Utveckla U-SQL-program</span><span class="sxs-lookup"><span data-stu-id="67905-132">Develop U-SQL application</span></span>
<span data-ttu-id="67905-133">Ett U-SQL-program är oftast ett U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="67905-133">A U-SQL application is mostly a U-SQL script.</span></span> <span data-ttu-id="67905-134">Mer information om U-SQL finns [Kom igång med U-SQL](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="67905-134">To learn more about U-SQL, see [Get started with U-SQL](data-lake-analytics-u-sql-get-started.md).</span></span>

<span data-ttu-id="67905-135">Du kan lägga till ytterligare användardefinierade operatorer till programmet.</span><span class="sxs-lookup"><span data-stu-id="67905-135">You can add addition user-defined operators to the application.</span></span>  <span data-ttu-id="67905-136">Mer information finns i [utveckla U-SQL-operatörer för Data Lake Analytics-jobb för användardefinierade](data-lake-analytics-u-sql-develop-user-defined-operators.md).</span><span class="sxs-lookup"><span data-stu-id="67905-136">For more information, see [Develop U-SQL user defined operators for Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-user-defined-operators.md).</span></span>

<span data-ttu-id="67905-137">**Skapa och skicka ett Data Lake Analytics-jobb**</span><span class="sxs-lookup"><span data-stu-id="67905-137">**To create and submit a Data Lake Analytics job**</span></span>

1. <span data-ttu-id="67905-138">Klickar du på den **Arkiv > Nytt > projekt**.</span><span class="sxs-lookup"><span data-stu-id="67905-138">Click the **File > New > Project**.</span></span>
2. <span data-ttu-id="67905-139">Välj typ av U-SQL-projekt.</span><span class="sxs-lookup"><span data-stu-id="67905-139">Select the U-SQL Project type.</span></span>

    ![nytt U-SQL Visual Studio-projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. <span data-ttu-id="67905-141">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="67905-141">Click **OK**.</span></span> <span data-ttu-id="67905-142">Visual studio skapar en lösning med en Script.usql-fil.</span><span class="sxs-lookup"><span data-stu-id="67905-142">Visual studio creates a solution with a Script.usql file.</span></span>
4. <span data-ttu-id="67905-143">Ange följande skript i filen Script.usql:</span><span class="sxs-lookup"><span data-stu-id="67905-143">Enter the following script into the Script.usql file:</span></span>

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
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

    <span data-ttu-id="67905-144">Information om U-SQL finns [Kom igång med Data Lake Analytics U-SQL-språket](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="67905-144">To understand the U-SQL, see [Get started with Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>    
5. <span data-ttu-id="67905-145">Lägg till ett nytt U-SQL-skript i projektet och ange följande:</span><span class="sxs-lookup"><span data-stu-id="67905-145">Add a new U-SQL script to your project and enter the following:</span></span>

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();
6. <span data-ttu-id="67905-146">Växla tillbaka till det första U-SQL-skriptet och bredvid den **skicka** knappen, ange ditt Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="67905-146">Switch back to the first U-SQL script and next to the **Submit** button, specify your Analytics account.</span></span>
7. <span data-ttu-id="67905-147">Från **Solution Explorer**, högerklicka på **Script.usql** och klicka sedan på **Skapa skript**.</span><span class="sxs-lookup"><span data-stu-id="67905-147">From **Solution Explorer**, right click **Script.usql**, and then click **Build Script**.</span></span> <span data-ttu-id="67905-148">Kontrollera resultatet i utdatarutan.</span><span class="sxs-lookup"><span data-stu-id="67905-148">Verify the results in the Output pane.</span></span>
8. <span data-ttu-id="67905-149">Från **Solution Explorer**, högerklicka på **Script.usql** och klicka sedan på **Skicka skript**.</span><span class="sxs-lookup"><span data-stu-id="67905-149">From **Solution Explorer**, right click **Script.usql**, and then click **Submit Script**.</span></span>
9. <span data-ttu-id="67905-150">Kontrollera den **Analytics-konto** är där du vill köra jobbet och klicka sedan på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="67905-150">Verify the **Analytics Account** is the one where you want to run the job, and then click **Submit**.</span></span> <span data-ttu-id="67905-151">Resultat för skicka och jobblänk är tillgängliga i resultatfönstret för Data Lake-verktyg för Visual Studio när överföringen är klar.</span><span class="sxs-lookup"><span data-stu-id="67905-151">Submission results and job link are available in the Data Lake Tools for Visual Studio Results window when the submission is completed.</span></span>
10. <span data-ttu-id="67905-152">Vänta tills jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="67905-152">Wait until the job is completed successfully.</span></span>  <span data-ttu-id="67905-153">Om jobbet inte saknar troligen källfilen.</span><span class="sxs-lookup"><span data-stu-id="67905-153">If the job failed, it is most likely missing the source file.</span></span>  <span data-ttu-id="67905-154">Se avsnittet förutsättningar i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="67905-154">Please see the Prerequisite section of this tutorial.</span></span> <span data-ttu-id="67905-155">Ytterligare felsökningsinformation finns i [övervaka och felsöka Azure Data Lake Analytics-jobb](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="67905-155">For additional troubleshooting information, see [Monitor and troubleshoot Azure Data Lake Analytics jobs](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span></span>

    <span data-ttu-id="67905-156">När jobbet är slutfört, bör du se följande skärm:</span><span class="sxs-lookup"><span data-stu-id="67905-156">When the job is completed, you shall see the following screen:</span></span>

    ![datasjöanalys analysera webbloggar webbplatsloggar](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)
11. <span data-ttu-id="67905-158">Upprepa steg 7 – 10 för **Script1.usql**.</span><span class="sxs-lookup"><span data-stu-id="67905-158">Now repeat steps 7- 10 for **Script1.usql**.</span></span>

<span data-ttu-id="67905-159">**Visa jobbutdata**</span><span class="sxs-lookup"><span data-stu-id="67905-159">**To see the job output**</span></span>

1. <span data-ttu-id="67905-160">Från **Server Explorer**, expandera **Azure**, expandera **Data Lake Analytics**, expandera Data Lake Analytics-kontot, expandera **Lagringskonton**, högerklicka på standardkontot för lagring av Data Lake och klicka sedan på **Explorer**.</span><span class="sxs-lookup"><span data-stu-id="67905-160">From **Server Explorer**, expand **Azure**, expand **Data Lake Analytics**, expand your Data Lake Analytics account, expand **Storage Accounts**, right-click the default Data Lake Storage account, and then click **Explorer**.</span></span>
2. <span data-ttu-id="67905-161">Dubbelklicka på **exempel** att öppna mappen och dubbelklickar på **utdata**.</span><span class="sxs-lookup"><span data-stu-id="67905-161">Double-click **Samples** to open the folder, and then double-click **Outputs**.</span></span>
3. <span data-ttu-id="67905-162">Dubbelklicka på **UnsuccessfulResponsees.log**.</span><span class="sxs-lookup"><span data-stu-id="67905-162">Double-click **UnsuccessfulResponsees.log**.</span></span>
4. <span data-ttu-id="67905-163">Du kan också dubbelklicka på filen i diagramvyn för jobbet för att kunna gå direkt till utdata.</span><span class="sxs-lookup"><span data-stu-id="67905-163">You can also double-click the output file inside the graph view of the job in order to navigate directly to the output.</span></span>

## <a name="see-also"></a><span data-ttu-id="67905-164">Se även</span><span class="sxs-lookup"><span data-stu-id="67905-164">See also</span></span>
<span data-ttu-id="67905-165">Om du vill komma igång med Data Lake Analytics med hjälp av olika verktyg, se:</span><span class="sxs-lookup"><span data-stu-id="67905-165">To get started with Data Lake Analytics using different tools, see:</span></span>

* [<span data-ttu-id="67905-166">Kom igång med Data Lake Analytics med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="67905-166">Get started with Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="67905-167">Kom igång med Data Lake Analytics med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="67905-167">Get started with Data Lake Analytics using Azure PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="67905-168">Kom igång med Data Lake Analytics med hjälp av NET SDK</span><span class="sxs-lookup"><span data-stu-id="67905-168">Get started with Data Lake Analytics using .NET SDK</span></span>](data-lake-analytics-get-started-net-sdk.md)
