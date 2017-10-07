---
title: "aaaAnalyze data i Data Lake Store med hjälp av Power BI | Microsoft Docs"
description: "Använd Power BI tooanalyze data som lagras i Azure Data Lake Store"
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
ms.openlocfilehash: 6a1bfa80fd1b0dda59b7eaaae9ca1585ba42783e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a><span data-ttu-id="abaec-103">Analysera data i Data Lake Store med hjälp av Power BI</span><span class="sxs-lookup"><span data-stu-id="abaec-103">Analyze data in Data Lake Store by using Power BI</span></span>
<span data-ttu-id="abaec-104">I den här artikeln får du lära dig hur toouse Power BI Desktop tooanalyze och visualisera data som lagras i Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="abaec-104">In this article you will learn how toouse Power BI Desktop tooanalyze and visualize data stored in Azure Data Lake Store.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="abaec-105">Krav</span><span class="sxs-lookup"><span data-stu-id="abaec-105">Prerequisites</span></span>
<span data-ttu-id="abaec-106">Innan du påbörjar den här självstudien måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="abaec-106">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="abaec-107">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="abaec-107">**An Azure subscription**.</span></span> <span data-ttu-id="abaec-108">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="abaec-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="abaec-109">**Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="abaec-109">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="abaec-110">Följ instruktionerna för hello på [Kom igång med Azure Data Lake Store med hjälp av hello Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="abaec-110">Follow hello instructions at [Get started with Azure Data Lake Store using hello Azure Portal](data-lake-store-get-started-portal.md).</span></span> <span data-ttu-id="abaec-111">Den här artikeln förutsätter att du redan har skapat ett Data Lake Store-konto, som kallas **mybidatalakestore**, och överföra en exempeldatafil (**Drivers.txt**) tooit.</span><span class="sxs-lookup"><span data-stu-id="abaec-111">This article assumes that you have already created a Data Lake Store account, called **mybidatalakestore**, and uploaded a sample data file (**Drivers.txt**) tooit.</span></span> <span data-ttu-id="abaec-112">Den här exempelfilen är tillgängliga för nedladdning från [Azure Data Lake Git-lagringsplatsen](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span><span class="sxs-lookup"><span data-stu-id="abaec-112">This sample file is available for download from [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span></span>
* <span data-ttu-id="abaec-113">**Power BI Desktop**.</span><span class="sxs-lookup"><span data-stu-id="abaec-113">**Power BI Desktop**.</span></span> <span data-ttu-id="abaec-114">Du kan ladda ned det från [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331).</span><span class="sxs-lookup"><span data-stu-id="abaec-114">You can download this from [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331).</span></span> 

## <a name="create-a-report-in-power-bi-desktop"></a><span data-ttu-id="abaec-115">Skapa en rapport i Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="abaec-115">Create a report in Power BI Desktop</span></span>
1. <span data-ttu-id="abaec-116">Starta Power BI Desktop på datorn.</span><span class="sxs-lookup"><span data-stu-id="abaec-116">Launch Power BI Desktop on your computer.</span></span>
2. <span data-ttu-id="abaec-117">Från hello **Start** band klickar du på **hämta Data**, och klicka sedan på mer.</span><span class="sxs-lookup"><span data-stu-id="abaec-117">From hello **Home** ribbon, click **Get Data**, and then click More.</span></span> <span data-ttu-id="abaec-118">I hello **hämta Data** dialogrutan klickar du på **Azure**, klickar du på **Azure Data Lake Store**, och klicka sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="abaec-118">In hello **Get Data** dialog box, click **Azure**, click **Azure Data Lake Store**, and then click **Connect**.</span></span>
   
    <span data-ttu-id="abaec-119">![Anslut tooData Datasjölager](./media/data-lake-store-power-bi/get-data-lake-store-account.png "Anslut tooData Lake Store")</span><span class="sxs-lookup"><span data-stu-id="abaec-119">![Connect tooData Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account.png "Connect tooData Lake Store")</span></span>
3. <span data-ttu-id="abaec-120">Om du ser en dialogruta om hello connector som i en utvecklingsfasen, välja toocontinue.</span><span class="sxs-lookup"><span data-stu-id="abaec-120">If you see a dialog box about hello connector being in a development phase, opt toocontinue.</span></span>
4. <span data-ttu-id="abaec-121">I hello **Microsoft Azure Data Lake Store** dialogrutan Ange hello URL tooyour Data Lake Store-konto och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="abaec-121">In hello **Microsoft Azure Data Lake Store** dialog box, provide hello URL tooyour Data Lake Store account, and then click **OK**.</span></span>
   
    <span data-ttu-id="abaec-122">![URL för Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL för Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="abaec-122">![URL for Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL for Data Lake Store")</span></span>
5. <span data-ttu-id="abaec-123">Hello nästa i dialogrutan klickar du på **logga in** toosign till Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="abaec-123">In hello next dialog box, click **Sign in** toosign into Data Lake Store account.</span></span> <span data-ttu-id="abaec-124">Du omdirigeras tooyour organisation inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="abaec-124">You will be redirected tooyour organization's sign in page.</span></span> <span data-ttu-id="abaec-125">Följ hello prompter toosign hello hänsyn.</span><span class="sxs-lookup"><span data-stu-id="abaec-125">Follow hello prompts toosign into hello account.</span></span>
   
    <span data-ttu-id="abaec-126">![Logga in på Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "logga in på Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="abaec-126">![Sign into Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Sign into Data Lake Store")</span></span>
6. <span data-ttu-id="abaec-127">När du har loggat in, klickar du på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="abaec-127">After you have successfully signed in, click **Connect**.</span></span>
   
    <span data-ttu-id="abaec-128">![Anslut tooData Datasjölager](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Anslut tooData Lake Store")</span><span class="sxs-lookup"><span data-stu-id="abaec-128">![Connect tooData Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Connect tooData Lake Store")</span></span>
7. <span data-ttu-id="abaec-129">hello nästa dialogruta visar hello filen du överförde tooyour Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="abaec-129">hello next dialog box shows hello file that you uploaded tooyour Data Lake Store account.</span></span> <span data-ttu-id="abaec-130">Kontrollera hello information och klickar sedan på **belastningen**.</span><span class="sxs-lookup"><span data-stu-id="abaec-130">Verify hello info and then click **Load**.</span></span>
   
    <span data-ttu-id="abaec-131">![Läs in data från Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "läsa in data från Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="abaec-131">![Load data from Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Load data from Data Lake Store")</span></span>
8. <span data-ttu-id="abaec-132">När hello data har lästs in Power BI, visas följande fält i hello hello **fält** fliken.</span><span class="sxs-lookup"><span data-stu-id="abaec-132">After hello data has been successfully loaded into Power BI, you will see hello following fields in hello **Fields** tab.</span></span>
   
    <span data-ttu-id="abaec-133">![Importera fält](./media/data-lake-store-power-bi/imported-fields.png "importeras fält")</span><span class="sxs-lookup"><span data-stu-id="abaec-133">![Imported fields](./media/data-lake-store-power-bi/imported-fields.png "Imported fields")</span></span>
   
    <span data-ttu-id="abaec-134">Dock toovisualize och analysera hello data, vi föredrar hello data toobe tillgängliga per hello följande fält</span><span class="sxs-lookup"><span data-stu-id="abaec-134">However, toovisualize and analyze hello data, we prefer hello data toobe available per hello following fields</span></span>
   
    <span data-ttu-id="abaec-135">![Önskade fält](./media/data-lake-store-power-bi/desired-fields.png "önskade fält")</span><span class="sxs-lookup"><span data-stu-id="abaec-135">![Desired fields](./media/data-lake-store-power-bi/desired-fields.png "Desired fields")</span></span>
   
    <span data-ttu-id="abaec-136">I hello nästa steg ska uppdaterar vi hello frågan tooconvert hello importerade data i hello önskade format.</span><span class="sxs-lookup"><span data-stu-id="abaec-136">In hello next steps, we will update hello query tooconvert hello imported data in hello desired format.</span></span>
9. <span data-ttu-id="abaec-137">Från hello **Start** band klickar du på **redigera frågor**.</span><span class="sxs-lookup"><span data-stu-id="abaec-137">From hello **Home** ribbon, click **Edit Queries**.</span></span>
   
    <span data-ttu-id="abaec-138">![Redigera frågor](./media/data-lake-store-power-bi/edit-queries.png "redigera frågor")</span><span class="sxs-lookup"><span data-stu-id="abaec-138">![Edit queries](./media/data-lake-store-power-bi/edit-queries.png "Edit queries")</span></span>
10. <span data-ttu-id="abaec-139">I hello Query Editor under hello **innehåll** kolumnen, klickar du på **binär**.</span><span class="sxs-lookup"><span data-stu-id="abaec-139">In hello Query Editor, under hello **Content** column, click **Binary**.</span></span>
    
    <span data-ttu-id="abaec-140">![Redigera frågor](./media/data-lake-store-power-bi/convert-query1.png "redigera frågor")</span><span class="sxs-lookup"><span data-stu-id="abaec-140">![Edit queries](./media/data-lake-store-power-bi/convert-query1.png "Edit queries")</span></span>
11. <span data-ttu-id="abaec-141">Du ser en filikon som representerar hello **Drivers.txt** -fil som du har överfört.</span><span class="sxs-lookup"><span data-stu-id="abaec-141">You will see a file icon, that represents hello **Drivers.txt** file that you uploaded.</span></span> <span data-ttu-id="abaec-142">Högerklicka på hello-filen och klicka på **CSV**.</span><span class="sxs-lookup"><span data-stu-id="abaec-142">Right-click hello file, and click **CSV**.</span></span>    
    
    <span data-ttu-id="abaec-143">![Redigera frågor](./media/data-lake-store-power-bi/convert-query2.png "redigera frågor")</span><span class="sxs-lookup"><span data-stu-id="abaec-143">![Edit queries](./media/data-lake-store-power-bi/convert-query2.png "Edit queries")</span></span>
12. <span data-ttu-id="abaec-144">Du bör se utdata som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="abaec-144">You should see an output as shown below.</span></span> <span data-ttu-id="abaec-145">Dina data finns nu i ett format som du kan använda toocreate visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="abaec-145">Your data is now available in a format that you can use toocreate visualizations.</span></span>
    
    <span data-ttu-id="abaec-146">![Redigera frågor](./media/data-lake-store-power-bi/convert-query3.png "redigera frågor")</span><span class="sxs-lookup"><span data-stu-id="abaec-146">![Edit queries](./media/data-lake-store-power-bi/convert-query3.png "Edit queries")</span></span>
13. <span data-ttu-id="abaec-147">Från hello **Start** band klickar du på **stängs och**, och klicka sedan på **stängs och**.</span><span class="sxs-lookup"><span data-stu-id="abaec-147">From hello **Home** ribbon, click **Close and Apply**, and then click **Close and Apply**.</span></span>
    
    <span data-ttu-id="abaec-148">![Redigera frågor](./media/data-lake-store-power-bi/load-edited-query.png "redigera frågor")</span><span class="sxs-lookup"><span data-stu-id="abaec-148">![Edit queries](./media/data-lake-store-power-bi/load-edited-query.png "Edit queries")</span></span>
14. <span data-ttu-id="abaec-149">När hello frågan uppdateras hello **fält** fliken visar hello nya fält som är tillgängliga för visualisering.</span><span class="sxs-lookup"><span data-stu-id="abaec-149">Once hello query is updated, hello **Fields** tab will show hello new fields available for visualization.</span></span>
    
    <span data-ttu-id="abaec-150">![Uppdatera fält](./media/data-lake-store-power-bi/updated-query-fields.png "uppdatera fält")</span><span class="sxs-lookup"><span data-stu-id="abaec-150">![Updated fields](./media/data-lake-store-power-bi/updated-query-fields.png "Updated fields")</span></span>
15. <span data-ttu-id="abaec-151">Låt oss skapa ett cirkeldiagram toorepresent hello drivrutiner i varje ort för ett visst land.</span><span class="sxs-lookup"><span data-stu-id="abaec-151">Let us create a pie chart toorepresent hello drivers in each city for a given country.</span></span> <span data-ttu-id="abaec-152">toodo, se hello följande val.</span><span class="sxs-lookup"><span data-stu-id="abaec-152">toodo so, make hello following selections.</span></span>
    
    1. <span data-ttu-id="abaec-153">Klicka på hello symbolen för ett cirkeldiagram hello visualiseringar på fliken.</span><span class="sxs-lookup"><span data-stu-id="abaec-153">From hello Visualizations tab, click hello symbol for a pie chart.</span></span>
       
        <span data-ttu-id="abaec-154">![Skapa cirkeldiagram](./media/data-lake-store-power-bi/create-pie-chart.png "skapa cirkeldiagram")</span><span class="sxs-lookup"><span data-stu-id="abaec-154">![Create pie chart](./media/data-lake-store-power-bi/create-pie-chart.png "Create pie chart")</span></span>
    2. <span data-ttu-id="abaec-155">hello kolumner som vi toouse är **kolumner 4** (namn på hello ort) och **kolumn 7** (namnet på hello land).</span><span class="sxs-lookup"><span data-stu-id="abaec-155">hello columns that we are going toouse are **Column 4** (name of hello city) and **Column 7** (name of hello country).</span></span> <span data-ttu-id="abaec-156">Dra dessa kolumner från **fält** fliken för**visualiseringar** fliken enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="abaec-156">Drag these columns from **Fields** tab too**Visualizations** tab as shown below.</span></span>
       
        <span data-ttu-id="abaec-157">![Skapa grafik](./media/data-lake-store-power-bi/create-visualizations.png "skapa grafik")</span><span class="sxs-lookup"><span data-stu-id="abaec-157">![Create visualizations](./media/data-lake-store-power-bi/create-visualizations.png "Create visualizations")</span></span>
    3. <span data-ttu-id="abaec-158">hello cirkeldiagram bör nu se ut ungefär som hello som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="abaec-158">hello pie chart should now resemble like hello one shown below.</span></span>
       
        <span data-ttu-id="abaec-159">![Cirkeldiagram](./media/data-lake-store-power-bi/pie-chart.png "skapa grafik")</span><span class="sxs-lookup"><span data-stu-id="abaec-159">![Pie chart](./media/data-lake-store-power-bi/pie-chart.png "Create visualizations")</span></span>
16. <span data-ttu-id="abaec-160">Du kan nu se hello antal drivrutiner i varje ort i hello valt land genom att välja ett visst land från hello sidfilter nivå.</span><span class="sxs-lookup"><span data-stu-id="abaec-160">By selecting a specific country from hello page level filters, you can now see hello number of drivers in each city of hello selected country.</span></span> <span data-ttu-id="abaec-161">Till exempel under hello **visualiseringar** fliken, under **sidan nivå filter**väljer **Brasilien**.</span><span class="sxs-lookup"><span data-stu-id="abaec-161">For example, under hello **Visualizations** tab, under **Page level filters**, select **Brazil**.</span></span>
    
    <span data-ttu-id="abaec-162">![Välj ett land](./media/data-lake-store-power-bi/select-country.png "Välj land")</span><span class="sxs-lookup"><span data-stu-id="abaec-162">![Select a country](./media/data-lake-store-power-bi/select-country.png "Select a country")</span></span>
17. <span data-ttu-id="abaec-163">hello cirkeldiagram är uppdateras automatiskt toodisplay hello drivrutiner i hello orter av Brasilien.</span><span class="sxs-lookup"><span data-stu-id="abaec-163">hello pie chart is automatically updated toodisplay hello drivers in hello cities of Brazil.</span></span>
    
    <span data-ttu-id="abaec-164">![Drivrutiner i ett land](./media/data-lake-store-power-bi/driver-per-country.png "drivrutiner per land")</span><span class="sxs-lookup"><span data-stu-id="abaec-164">![Drivers in a country](./media/data-lake-store-power-bi/driver-per-country.png "Drivers per country")</span></span>
18. <span data-ttu-id="abaec-165">Från hello **filen** -menyn klickar du på **spara** toosave hello visualiseringen som en Power BI Desktop-fil.</span><span class="sxs-lookup"><span data-stu-id="abaec-165">From hello **File** menu, click **Save** toosave hello visualization as a Power BI Desktop file.</span></span>

## <a name="publish-report-toopower-bi-service"></a><span data-ttu-id="abaec-166">Publicera rapporten tooPower BI-tjänsten</span><span class="sxs-lookup"><span data-stu-id="abaec-166">Publish report tooPower BI service</span></span>
<span data-ttu-id="abaec-167">När du har skapat hello visualiseringar i Power BI Desktop kan dela du den med andra genom att publicera den toohello Power BI-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="abaec-167">Once you have created hello visualizations in Power BI Desktop, you can share it with others by publishing it toohello Power BI service.</span></span> <span data-ttu-id="abaec-168">Anvisningar för hur toodo som finns i [publicera från Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).</span><span class="sxs-lookup"><span data-stu-id="abaec-168">For instructions on how toodo that, see [Publish from Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).</span></span>

## <a name="see-also"></a><span data-ttu-id="abaec-169">Se även</span><span class="sxs-lookup"><span data-stu-id="abaec-169">See also</span></span>
* [<span data-ttu-id="abaec-170">Analysera data i Data Lake Store med hjälp av Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="abaec-170">Analyze data in Data Lake Store using Data Lake Analytics</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

