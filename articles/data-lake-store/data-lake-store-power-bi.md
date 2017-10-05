---
title: "Analysera data i Data Lake Store med hjälp av Power BI | Microsoft Docs"
description: "Använd Power BI för att analysera data som lagras i Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57d19d27-e135-49d9-a7ea-46c48ef4e3bd
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 0cf7e385ef2edd650479e120f52469bc6632f2eb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a><span data-ttu-id="167e8-103">Analysera data i Data Lake Store med hjälp av Power BI</span><span class="sxs-lookup"><span data-stu-id="167e8-103">Analyze data in Data Lake Store by using Power BI</span></span>
<span data-ttu-id="167e8-104">I den här artikeln får du lära dig hur du använder Power BI Desktop att analysera och visualisera data som lagras i Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="167e8-104">In this article you will learn how to use Power BI Desktop to analyze and visualize data stored in Azure Data Lake Store.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="167e8-105">Krav</span><span class="sxs-lookup"><span data-stu-id="167e8-105">Prerequisites</span></span>
<span data-ttu-id="167e8-106">Innan du påbörjar de här självstudierna måste du ha:</span><span class="sxs-lookup"><span data-stu-id="167e8-106">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="167e8-107">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="167e8-107">**An Azure subscription**.</span></span> <span data-ttu-id="167e8-108">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="167e8-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="167e8-109">**Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="167e8-109">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="167e8-110">Följ instruktionerna i [Kom igång med Azure Data Lake Store med hjälp av Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="167e8-110">Follow the instructions at [Get started with Azure Data Lake Store using the Azure Portal](data-lake-store-get-started-portal.md).</span></span> <span data-ttu-id="167e8-111">Den här artikeln förutsätter att du redan har skapat ett Data Lake Store-konto, som kallas **mybidatalakestore**, och överföra en exempeldatafil (**Drivers.txt**) till den.</span><span class="sxs-lookup"><span data-stu-id="167e8-111">This article assumes that you have already created a Data Lake Store account, called **mybidatalakestore**, and uploaded a sample data file (**Drivers.txt**) to it.</span></span> <span data-ttu-id="167e8-112">Den här exempelfilen är tillgängliga för nedladdning från [Azure Data Lake Git-lagringsplatsen](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span><span class="sxs-lookup"><span data-stu-id="167e8-112">This sample file is available for download from [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span></span>
* <span data-ttu-id="167e8-113">**Power BI Desktop**.</span><span class="sxs-lookup"><span data-stu-id="167e8-113">**Power BI Desktop**.</span></span> <span data-ttu-id="167e8-114">Du kan ladda ned det från [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331).</span><span class="sxs-lookup"><span data-stu-id="167e8-114">You can download this from [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331).</span></span> 

## <a name="create-a-report-in-power-bi-desktop"></a><span data-ttu-id="167e8-115">Skapa en rapport i Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="167e8-115">Create a report in Power BI Desktop</span></span>
1. <span data-ttu-id="167e8-116">Starta Power BI Desktop på datorn.</span><span class="sxs-lookup"><span data-stu-id="167e8-116">Launch Power BI Desktop on your computer.</span></span>
2. <span data-ttu-id="167e8-117">Från den **Start** band klickar du på **hämta Data**, och klicka sedan på mer.</span><span class="sxs-lookup"><span data-stu-id="167e8-117">From the **Home** ribbon, click **Get Data**, and then click More.</span></span> <span data-ttu-id="167e8-118">I den **hämta Data** dialogrutan klickar du på **Azure**, klickar du på **Azure Data Lake Store**, och klicka sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="167e8-118">In the **Get Data** dialog box, click **Azure**, click **Azure Data Lake Store**, and then click **Connect**.</span></span>
   
    <span data-ttu-id="167e8-119">![Anslut till Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account.png "ansluta till Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="167e8-119">![Connect to Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account.png "Connect to Data Lake Store")</span></span>
3. <span data-ttu-id="167e8-120">Om du ser en dialogruta om att den finns i en utvecklingsfasen connector, välja för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="167e8-120">If you see a dialog box about the connector being in a development phase, opt to continue.</span></span>
4. <span data-ttu-id="167e8-121">I den **Microsoft Azure Data Lake Store** dialogrutan Ange URL till ditt Data Lake Store-konto och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="167e8-121">In the **Microsoft Azure Data Lake Store** dialog box, provide the URL to your Data Lake Store account, and then click **OK**.</span></span>
   
    <span data-ttu-id="167e8-122">![URL för Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL för Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="167e8-122">![URL for Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL for Data Lake Store")</span></span>
5. <span data-ttu-id="167e8-123">Klicka på nästa dialogrutan **inloggning** att logga in på Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="167e8-123">In the next dialog box, click **Sign in** to sign into Data Lake Store account.</span></span> <span data-ttu-id="167e8-124">Du omdirigeras till inloggningssidan för din organisation.</span><span class="sxs-lookup"><span data-stu-id="167e8-124">You will be redirected to your organization's sign in page.</span></span> <span data-ttu-id="167e8-125">Följ anvisningarna för att logga in på kontot.</span><span class="sxs-lookup"><span data-stu-id="167e8-125">Follow the prompts to sign into the account.</span></span>
   
    <span data-ttu-id="167e8-126">![Logga in på Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "logga in på Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="167e8-126">![Sign into Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Sign into Data Lake Store")</span></span>
6. <span data-ttu-id="167e8-127">När du har loggat in, klickar du på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="167e8-127">After you have successfully signed in, click **Connect**.</span></span>
   
    <span data-ttu-id="167e8-128">![Anslut till Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "ansluta till Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="167e8-128">![Connect to Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Connect to Data Lake Store")</span></span>
7. <span data-ttu-id="167e8-129">Nästa dialogruta visar den fil som du överfört till ditt Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="167e8-129">The next dialog box shows the file that you uploaded to your Data Lake Store account.</span></span> <span data-ttu-id="167e8-130">Kontrollera informationen och klicka sedan på **belastningen**.</span><span class="sxs-lookup"><span data-stu-id="167e8-130">Verify the info and then click **Load**.</span></span>
   
    <span data-ttu-id="167e8-131">![Läs in data från Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "läsa in data från Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="167e8-131">![Load data from Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Load data from Data Lake Store")</span></span>
8. <span data-ttu-id="167e8-132">När data har lästs in Power BI, visas följande fält i den **fält** fliken.</span><span class="sxs-lookup"><span data-stu-id="167e8-132">After the data has been successfully loaded into Power BI, you will see the following fields in the **Fields** tab.</span></span>
   
    <span data-ttu-id="167e8-133">![Importera fält](./media/data-lake-store-power-bi/imported-fields.png "importeras fält")</span><span class="sxs-lookup"><span data-stu-id="167e8-133">![Imported fields](./media/data-lake-store-power-bi/imported-fields.png "Imported fields")</span></span>
   
    <span data-ttu-id="167e8-134">Om du vill visualisera och analysera data, föredra vi dock data ska vara tillgängliga per följande fält</span><span class="sxs-lookup"><span data-stu-id="167e8-134">However, to visualize and analyze the data, we prefer the data to be available per the following fields</span></span>
   
    <span data-ttu-id="167e8-135">![Önskade fält](./media/data-lake-store-power-bi/desired-fields.png "önskade fält")</span><span class="sxs-lookup"><span data-stu-id="167e8-135">![Desired fields](./media/data-lake-store-power-bi/desired-fields.png "Desired fields")</span></span>
   
    <span data-ttu-id="167e8-136">I nästa steg ska vi kommer att uppdatera frågan om du vill konvertera den importerade informationen i det önskade formatet.</span><span class="sxs-lookup"><span data-stu-id="167e8-136">In the next steps, we will update the query to convert the imported data in the desired format.</span></span>
9. <span data-ttu-id="167e8-137">Från den **Start** band klickar du på **redigera frågor**.</span><span class="sxs-lookup"><span data-stu-id="167e8-137">From the **Home** ribbon, click **Edit Queries**.</span></span>
   
    <span data-ttu-id="167e8-138">![Redigera frågor](./media/data-lake-store-power-bi/edit-queries.png "redigera frågor")</span><span class="sxs-lookup"><span data-stu-id="167e8-138">![Edit queries](./media/data-lake-store-power-bi/edit-queries.png "Edit queries")</span></span>
10. <span data-ttu-id="167e8-139">I frågeredigeraren under den **innehåll** kolumnen, klickar du på **binär**.</span><span class="sxs-lookup"><span data-stu-id="167e8-139">In the Query Editor, under the **Content** column, click **Binary**.</span></span>
    
    <span data-ttu-id="167e8-140">![Redigera frågor](./media/data-lake-store-power-bi/convert-query1.png "redigera frågor")</span><span class="sxs-lookup"><span data-stu-id="167e8-140">![Edit queries](./media/data-lake-store-power-bi/convert-query1.png "Edit queries")</span></span>
11. <span data-ttu-id="167e8-141">Du ser en filikon som representerar den **Drivers.txt** -fil som du har överfört.</span><span class="sxs-lookup"><span data-stu-id="167e8-141">You will see a file icon, that represents the **Drivers.txt** file that you uploaded.</span></span> <span data-ttu-id="167e8-142">Högerklicka på filen och klicka på **CSV**.</span><span class="sxs-lookup"><span data-stu-id="167e8-142">Right-click the file, and click **CSV**.</span></span>    
    
    <span data-ttu-id="167e8-143">![Redigera frågor](./media/data-lake-store-power-bi/convert-query2.png "redigera frågor")</span><span class="sxs-lookup"><span data-stu-id="167e8-143">![Edit queries](./media/data-lake-store-power-bi/convert-query2.png "Edit queries")</span></span>
12. <span data-ttu-id="167e8-144">Du bör se utdata som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="167e8-144">You should see an output as shown below.</span></span> <span data-ttu-id="167e8-145">Dina data finns nu i ett format som du kan använda för att skapa visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="167e8-145">Your data is now available in a format that you can use to create visualizations.</span></span>
    
    <span data-ttu-id="167e8-146">![Redigera frågor](./media/data-lake-store-power-bi/convert-query3.png "redigera frågor")</span><span class="sxs-lookup"><span data-stu-id="167e8-146">![Edit queries](./media/data-lake-store-power-bi/convert-query3.png "Edit queries")</span></span>
13. <span data-ttu-id="167e8-147">Från den **Start** band klickar du på **stängs och**, och klicka sedan på **stängs och**.</span><span class="sxs-lookup"><span data-stu-id="167e8-147">From the **Home** ribbon, click **Close and Apply**, and then click **Close and Apply**.</span></span>
    
    <span data-ttu-id="167e8-148">![Redigera frågor](./media/data-lake-store-power-bi/load-edited-query.png "redigera frågor")</span><span class="sxs-lookup"><span data-stu-id="167e8-148">![Edit queries](./media/data-lake-store-power-bi/load-edited-query.png "Edit queries")</span></span>
14. <span data-ttu-id="167e8-149">När frågan uppdateras den **fält** fliken visas de nya fält som är tillgängliga för visualisering.</span><span class="sxs-lookup"><span data-stu-id="167e8-149">Once the query is updated, the **Fields** tab will show the new fields available for visualization.</span></span>
    
    <span data-ttu-id="167e8-150">![Uppdatera fält](./media/data-lake-store-power-bi/updated-query-fields.png "uppdatera fält")</span><span class="sxs-lookup"><span data-stu-id="167e8-150">![Updated fields](./media/data-lake-store-power-bi/updated-query-fields.png "Updated fields")</span></span>
15. <span data-ttu-id="167e8-151">Låt oss skapa ett cirkeldiagram som representerar drivrutinerna i varje ort för ett visst land.</span><span class="sxs-lookup"><span data-stu-id="167e8-151">Let us create a pie chart to represent the drivers in each city for a given country.</span></span> <span data-ttu-id="167e8-152">Om du vill göra det, gör du följande val.</span><span class="sxs-lookup"><span data-stu-id="167e8-152">To do so, make the following selections.</span></span>
    
    1. <span data-ttu-id="167e8-153">Klicka på symbolen för ett cirkeldiagram från fliken visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="167e8-153">From the Visualizations tab, click the symbol for a pie chart.</span></span>
       
        <span data-ttu-id="167e8-154">![Skapa cirkeldiagram](./media/data-lake-store-power-bi/create-pie-chart.png "skapa cirkeldiagram")</span><span class="sxs-lookup"><span data-stu-id="167e8-154">![Create pie chart](./media/data-lake-store-power-bi/create-pie-chart.png "Create pie chart")</span></span>
    2. <span data-ttu-id="167e8-155">De kolumner som vi ska använda är **kolumner 4** (namnet på staden) och **kolumn 7** (namnet på landet).</span><span class="sxs-lookup"><span data-stu-id="167e8-155">The columns that we are going to use are **Column 4** (name of the city) and **Column 7** (name of the country).</span></span> <span data-ttu-id="167e8-156">Dra dessa kolumner från **fält** fliken **visualiseringar** fliken enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="167e8-156">Drag these columns from **Fields** tab to **Visualizations** tab as shown below.</span></span>
       
        <span data-ttu-id="167e8-157">![Skapa grafik](./media/data-lake-store-power-bi/create-visualizations.png "skapa grafik")</span><span class="sxs-lookup"><span data-stu-id="167e8-157">![Create visualizations](./media/data-lake-store-power-bi/create-visualizations.png "Create visualizations")</span></span>
    3. <span data-ttu-id="167e8-158">Cirkeldiagrammet bör nu se ut ungefär som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="167e8-158">The pie chart should now resemble like the one shown below.</span></span>
       
        <span data-ttu-id="167e8-159">![Cirkeldiagram](./media/data-lake-store-power-bi/pie-chart.png "skapa grafik")</span><span class="sxs-lookup"><span data-stu-id="167e8-159">![Pie chart](./media/data-lake-store-power-bi/pie-chart.png "Create visualizations")</span></span>
16. <span data-ttu-id="167e8-160">Du kan nu se antalet drivrutiner i varje ort i det valda landet genom att välja ett visst land från nivån sidfilter.</span><span class="sxs-lookup"><span data-stu-id="167e8-160">By selecting a specific country from the page level filters, you can now see the number of drivers in each city of the selected country.</span></span> <span data-ttu-id="167e8-161">Under den **visualiseringar** fliken, under **sidan nivå filter**väljer **Brasilien**.</span><span class="sxs-lookup"><span data-stu-id="167e8-161">For example, under the **Visualizations** tab, under **Page level filters**, select **Brazil**.</span></span>
    
    <span data-ttu-id="167e8-162">![Välj ett land](./media/data-lake-store-power-bi/select-country.png "Välj land")</span><span class="sxs-lookup"><span data-stu-id="167e8-162">![Select a country](./media/data-lake-store-power-bi/select-country.png "Select a country")</span></span>
17. <span data-ttu-id="167e8-163">Cirkeldiagrammet uppdateras automatiskt för att visa drivrutinerna i Brasilien städer.</span><span class="sxs-lookup"><span data-stu-id="167e8-163">The pie chart is automatically updated to display the drivers in the cities of Brazil.</span></span>
    
    <span data-ttu-id="167e8-164">![Drivrutiner i ett land](./media/data-lake-store-power-bi/driver-per-country.png "drivrutiner per land")</span><span class="sxs-lookup"><span data-stu-id="167e8-164">![Drivers in a country](./media/data-lake-store-power-bi/driver-per-country.png "Drivers per country")</span></span>
18. <span data-ttu-id="167e8-165">Från den **filen** -menyn klickar du på **spara** spara visualiseringen som en Power BI Desktop-fil.</span><span class="sxs-lookup"><span data-stu-id="167e8-165">From the **File** menu, click **Save** to save the visualization as a Power BI Desktop file.</span></span>

## <a name="publish-report-to-power-bi-service"></a><span data-ttu-id="167e8-166">Publicera rapporten till Power BI-tjänsten</span><span class="sxs-lookup"><span data-stu-id="167e8-166">Publish report to Power BI service</span></span>
<span data-ttu-id="167e8-167">När du har skapat visualiseringar i Power BI Desktop kan dela du den med andra genom att publicera Power BI-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="167e8-167">Once you have created the visualizations in Power BI Desktop, you can share it with others by publishing it to the Power BI service.</span></span> <span data-ttu-id="167e8-168">Instruktioner om hur du gör finns [publicera från Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).</span><span class="sxs-lookup"><span data-stu-id="167e8-168">For instructions on how to do that, see [Publish from Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).</span></span>

## <a name="see-also"></a><span data-ttu-id="167e8-169">Se även</span><span class="sxs-lookup"><span data-stu-id="167e8-169">See also</span></span>
* [<span data-ttu-id="167e8-170">Analysera data i Data Lake Store med hjälp av Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="167e8-170">Analyze data in Data Lake Store using Data Lake Analytics</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

