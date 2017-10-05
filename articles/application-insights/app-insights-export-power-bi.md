---
title: "Exportera till Powerbi från Application Insights | Microsoft Docs"
description: "Analytics-frågor kan visas i Power BI."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: 350a65b1c6432baf258e014c9e63133d2b29e34f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="feed-power-bi-from-application-insights"></a><span data-ttu-id="912b2-103">Powerbi-feed från Application Insights</span><span class="sxs-lookup"><span data-stu-id="912b2-103">Feed Power BI from Application Insights</span></span>
<span data-ttu-id="912b2-104">[Power BI](http://www.powerbi.com/) är en uppsättning verktyg för business analytics som hjälper dig att analysera data och dela information.</span><span class="sxs-lookup"><span data-stu-id="912b2-104">[Power BI](http://www.powerbi.com/) is a suite of business analytics tools that help you analyze data and share insights.</span></span> <span data-ttu-id="912b2-105">Omfattande instrumentpaneler är tillgängliga på varje enhet.</span><span class="sxs-lookup"><span data-stu-id="912b2-105">Rich dashboards are available on every device.</span></span> <span data-ttu-id="912b2-106">Du kan kombinera data från flera källor, inklusive Analytics-frågor från [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="912b2-106">You can combine data from many sources, including Analytics queries from [Azure Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="912b2-107">Det finns tre metoder för att exportera Application Insights-data till Power BI.</span><span class="sxs-lookup"><span data-stu-id="912b2-107">There are three recommended methods of exporting Application Insights data to Power BI.</span></span> <span data-ttu-id="912b2-108">Du kan använda dem enskilt eller tillsammans.</span><span class="sxs-lookup"><span data-stu-id="912b2-108">You can use them separately or together.</span></span>

* <span data-ttu-id="912b2-109">[**Power BI kortet** ](#power-pi-adapter) -Ställ in en komplett instrumentpanel för telemetri från din app.</span><span class="sxs-lookup"><span data-stu-id="912b2-109">[**Power BI adapter**](#power-pi-adapter) - set up a complete dashboard of telemetry from your app.</span></span> <span data-ttu-id="912b2-110">Uppsättningen diagram är fördefinierade, men du kan lägga till egna frågor från andra källor.</span><span class="sxs-lookup"><span data-stu-id="912b2-110">The set of charts is predefined, but you can add your own queries from any other sources.</span></span>
* <span data-ttu-id="912b2-111">[**Exportera Analytics frågor** ](#export-analytics-queries) -skriva en fråga som du vill med hjälp av Analytics och exportera dem till Power BI.</span><span class="sxs-lookup"><span data-stu-id="912b2-111">[**Export Analytics queries**](#export-analytics-queries) - write any query you want using Analytics, and export it to Power BI.</span></span> <span data-ttu-id="912b2-112">Du kan placera den här frågan på en instrumentpanel tillsammans med andra data.</span><span class="sxs-lookup"><span data-stu-id="912b2-112">You can place this query on a dashboard along with any other data.</span></span>
* <span data-ttu-id="912b2-113">[**Den löpande exporten och Stream Analytics** ](app-insights-export-stream-analytics.md) -detta innebär mer arbete för att ställa in.</span><span class="sxs-lookup"><span data-stu-id="912b2-113">[**Continuous export and Stream Analytics**](app-insights-export-stream-analytics.md) - This involves more work to set up.</span></span> <span data-ttu-id="912b2-114">Det är användbart om du vill behålla dina data under långa perioder.</span><span class="sxs-lookup"><span data-stu-id="912b2-114">It is useful if you want to keep your data for long periods.</span></span> <span data-ttu-id="912b2-115">I annat fall rekommenderas de andra metoderna.</span><span class="sxs-lookup"><span data-stu-id="912b2-115">Otherwise, the other methods are recommended.</span></span>

## <a name="power-bi-adapter"></a><span data-ttu-id="912b2-116">Power BI-kort</span><span class="sxs-lookup"><span data-stu-id="912b2-116">Power BI adapter</span></span>
<span data-ttu-id="912b2-117">Den här metoden skapas en komplett instrumentpanel för telemetri.</span><span class="sxs-lookup"><span data-stu-id="912b2-117">This method creates a complete dashboard of telemetry for you.</span></span> <span data-ttu-id="912b2-118">Den första datamängden är fördefinierade, men du kan lägga till mer data till den.</span><span class="sxs-lookup"><span data-stu-id="912b2-118">The initial data set is predefined, but you can add more data to it.</span></span>

### <a name="get-the-adapter"></a><span data-ttu-id="912b2-119">Hämta kortet</span><span class="sxs-lookup"><span data-stu-id="912b2-119">Get the adapter</span></span>
1. <span data-ttu-id="912b2-120">Logga in på [Power BI](https://app.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="912b2-120">Sign in to [Power BI](https://app.powerbi.com/).</span></span>
2. <span data-ttu-id="912b2-121">Öppna **hämta Data**, **Services**, **Application Insights**</span><span class="sxs-lookup"><span data-stu-id="912b2-121">Open **Get Data**, **Services**, **Application Insights**</span></span>
   
    ![Hämta från Application Insights-datakälla](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. <span data-ttu-id="912b2-123">Ange information om Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="912b2-123">Provide the details of your Application Insights resource.</span></span>
   
    ![Hämta från Application Insights-datakälla](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. <span data-ttu-id="912b2-125">Vänta en minut eller två för data som ska importeras.</span><span class="sxs-lookup"><span data-stu-id="912b2-125">Wait a minute or two for the data to be imported.</span></span>
   
    ![Power BI-kort](./media/app-insights-export-power-bi/010.png)

<span data-ttu-id="912b2-127">Du kan redigera instrumentpanelen och kombinera Application Insights-diagram med de andra källor och med Analytics-frågor.</span><span class="sxs-lookup"><span data-stu-id="912b2-127">You can edit the dashboard, combining the Application Insights charts with those of other sources, and with Analytics queries.</span></span> <span data-ttu-id="912b2-128">Det finns ett visualiseringen galleri där du kan få fler diagram och varje diagram har en parametrar som du kan ange.</span><span class="sxs-lookup"><span data-stu-id="912b2-128">There's a visualization gallery where you can get more charts, and each chart has a parameters you can set.</span></span>

<span data-ttu-id="912b2-129">Efter importen fortsätter instrumentpanelen och rapporterna att uppdatera varje dag.</span><span class="sxs-lookup"><span data-stu-id="912b2-129">After the initial import, the dashboard and the reports continue to update daily.</span></span> <span data-ttu-id="912b2-130">Du kan styra uppdateringsschema för datamängden.</span><span class="sxs-lookup"><span data-stu-id="912b2-130">You can control the refresh schedule on the dataset.</span></span>

## <a name="export-analytics-queries"></a><span data-ttu-id="912b2-131">Exportera Analytics-frågor</span><span class="sxs-lookup"><span data-stu-id="912b2-131">Export Analytics queries</span></span>
<span data-ttu-id="912b2-132">Den här vägen kan du skriva alla Analytics-fråga som du vill och sedan exportera som en Power BI-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="912b2-132">This route allows you to write any Analytics query you like, and then export that to a Power BI dashboard.</span></span> <span data-ttu-id="912b2-133">(Du kan lägga till den instrumentpanel som har skapats av nätverkskort.)</span><span class="sxs-lookup"><span data-stu-id="912b2-133">(You can add to the dashboard created by the adapter.)</span></span>

### <a name="one-time-install-power-bi-desktop"></a><span data-ttu-id="912b2-134">En gång: Installera Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="912b2-134">One time: install Power BI Desktop</span></span>
<span data-ttu-id="912b2-135">Om du vill importera Application Insights frågan kan du använda skrivbordsversionen av Power BI.</span><span class="sxs-lookup"><span data-stu-id="912b2-135">To import your Application Insights query, you use the desktop version of Power BI.</span></span> <span data-ttu-id="912b2-136">Men sedan du kan publicera den på webben eller på din arbetsyta för Power BI-molnet.</span><span class="sxs-lookup"><span data-stu-id="912b2-136">But then you can publish it to the web or to your Power BI cloud workspace.</span></span> 

<span data-ttu-id="912b2-137">Installera [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).</span><span class="sxs-lookup"><span data-stu-id="912b2-137">Install [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).</span></span>

### <a name="export-an-analytics-query"></a><span data-ttu-id="912b2-138">Exportera en Analytics-fråga</span><span class="sxs-lookup"><span data-stu-id="912b2-138">Export an Analytics query</span></span>
1. <span data-ttu-id="912b2-139">[Öppna Analytics och skriva en fråga](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="912b2-139">[Open Analytics and write your query](app-insights-analytics-tour.md).</span></span>
2. <span data-ttu-id="912b2-140">Testa och förfina frågan tills du är nöjd med resultaten.</span><span class="sxs-lookup"><span data-stu-id="912b2-140">Test and refine the query until you're happy with the results.</span></span>

   <span data-ttu-id="912b2-141">**Kontrollera att frågan körs korrekt i Analytics innan du exporterar den.**</span><span class="sxs-lookup"><span data-stu-id="912b2-141">**Make sure that the query runs correctly in Analytics before you export it.**</span></span>
3. <span data-ttu-id="912b2-142">På den **exportera** -menyn, Välj **Power BI (M)**.</span><span class="sxs-lookup"><span data-stu-id="912b2-142">On the **Export** menu, choose **Power BI (M)**.</span></span> <span data-ttu-id="912b2-143">Spara textfilen.</span><span class="sxs-lookup"><span data-stu-id="912b2-143">Save the text file.</span></span>
   
    ![Exportera Power BI-fråga](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. <span data-ttu-id="912b2-145">I Power BI Desktop select **hämta Data, tom fråga** och sedan i frågeredigeraren för under **visa** Välj **avancerade frågeredigeraren**.</span><span class="sxs-lookup"><span data-stu-id="912b2-145">In Power BI Desktop select **Get Data, Blank Query** and then in the query editor, under **View** select **Advanced Query Editor**.</span></span>

    <span data-ttu-id="912b2-146">Klistra in skriptet exporterade M språk i avancerade frågeredigeraren.</span><span class="sxs-lookup"><span data-stu-id="912b2-146">Paste the exported M Language script into the Advanced Query Editor.</span></span>

    ![Avancerade frågeredigeraren](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. <span data-ttu-id="912b2-148">Du kan behöva ange autentiseringsuppgifter för att låta Power BI åtkomst till Azure.</span><span class="sxs-lookup"><span data-stu-id="912b2-148">You might have to provide credentials to allow Power BI to access Azure.</span></span> <span data-ttu-id="912b2-149">Använda 'organisationskonto' att logga in med ditt Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="912b2-149">Use 'organizational account' to sign in with your Microsoft account.</span></span>
   
    ![Ange autentiseringsuppgifter för Azure om du vill aktivera Power BI att köra frågan Application Insights](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    <span data-ttu-id="912b2-151">(Om du behöver verifiera autentiseringsuppgifterna kommandot inställningar för datakälla-menyn i frågeredigeraren.</span><span class="sxs-lookup"><span data-stu-id="912b2-151">(If you need to verify the credentials, use the Data Source Settings menu command in the Query Editor.</span></span> <span data-ttu-id="912b2-152">Var noga med för att ange de autentiseringsuppgifter som du använder för Azure, vilket kan skilja sig från dina autentiseringsuppgifter för Powerbi.)</span><span class="sxs-lookup"><span data-stu-id="912b2-152">Take care to specify the credentials you use for Azure, which might be different from your credentials for Power BI.)</span></span>
2. <span data-ttu-id="912b2-153">Välj en visualisering för din fråga och välj fält för x-axeln, y-axeln och segmentera dimension.</span><span class="sxs-lookup"><span data-stu-id="912b2-153">Choose a visualization for your query and select the fields for x-axis, y-axis, and segmenting dimension.</span></span>
   
    ![Välj visualiseringen](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. <span data-ttu-id="912b2-155">Publicera din rapporten till Power BI molnet arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="912b2-155">Publish your report to your Power BI cloud workspace.</span></span> <span data-ttu-id="912b2-156">Du kan bädda in en synkroniserade version till andra webbsidor därifrån.</span><span class="sxs-lookup"><span data-stu-id="912b2-156">From there, you can embed a synchronized version into other web pages.</span></span>
   
    ![Välj visualiseringen](./media/app-insights-export-power-bi/publish-power-bi.png)
4. <span data-ttu-id="912b2-158">Uppdatera rapporten manuellt med intervall eller konfigurera en schemalagd uppdatering på alternativsidan.</span><span class="sxs-lookup"><span data-stu-id="912b2-158">Refresh the report manually at intervals, or set up a scheduled refresh on the options page.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="912b2-159">Felsökning</span><span class="sxs-lookup"><span data-stu-id="912b2-159">Troubleshooting</span></span>

### <a name="401-or-403-unauthorized"></a><span data-ttu-id="912b2-160">401 eller 403 obehörig</span><span class="sxs-lookup"><span data-stu-id="912b2-160">401 or 403 Unauthorized</span></span> 
<span data-ttu-id="912b2-161">Detta kan inträffa om refesh-token inte har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="912b2-161">This can happen if your refesh token has not been updated.</span></span> <span data-ttu-id="912b2-162">Gör så här för att säkerställa att du har åtkomst.</span><span class="sxs-lookup"><span data-stu-id="912b2-162">Try these steps to ensure you still have access.</span></span> <span data-ttu-id="912b2-163">Om du har åtkomst till och refershing autentiseringsuppgifterna inte fungerar, öppna ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="912b2-163">If you do have access and refershing the credentials does not work, please open a support ticket.</span></span>

1. <span data-ttu-id="912b2-164">Logga in på Azure-portalen och kontrollera att du har åtkomst till resursen</span><span class="sxs-lookup"><span data-stu-id="912b2-164">Log into the Azure Portal and make sure you can access the resource</span></span>
2. <span data-ttu-id="912b2-165">Försök att uppdatera autentiseringsuppgifterna för instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="912b2-165">Try to refresh the credentials for the Dashboard</span></span>

### <a name="502-bad-gateway"></a><span data-ttu-id="912b2-166">502 felaktig Gateway</span><span class="sxs-lookup"><span data-stu-id="912b2-166">502 Bad Gateway</span></span>
<span data-ttu-id="912b2-167">Detta orsakas normalt av en Analytics-fråga som returnerar för mycket data.</span><span class="sxs-lookup"><span data-stu-id="912b2-167">This is usually caused by an Analytics query that returns too much data.</span></span> <span data-ttu-id="912b2-168">Du bör försöka använda en mindre tidsintervall eller genom att använda den [sedan](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) eller [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) fungerar endast [projekt](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) fält som du behöver.</span><span class="sxs-lookup"><span data-stu-id="912b2-168">You should try using a smaller time range or by using the [ago](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) or [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) functions only [project](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) the fields you need.</span></span>

<span data-ttu-id="912b2-169">Om att minska datauppsättningen kommer från Analytics-fråga inte uppfyller dina krav bör du använda den [API](https://dev.applicationinsights.io/documentation/overview) och hämtar en större datamängd.</span><span class="sxs-lookup"><span data-stu-id="912b2-169">If reducing the dataset coming from the Analytics query doesn't meet your requirements you should consider using the [API](https://dev.applicationinsights.io/documentation/overview) to pull a larger dataset.</span></span> <span data-ttu-id="912b2-170">Här följer instruktioner om hur du konverterar M-Query exporten att använda API: et.</span><span class="sxs-lookup"><span data-stu-id="912b2-170">Here are instructions on how to convert the M-Query export to use the API.</span></span>

1. <span data-ttu-id="912b2-171">Skapa en [API-nyckel](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)</span><span class="sxs-lookup"><span data-stu-id="912b2-171">Create an [API Key](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)</span></span>
2. <span data-ttu-id="912b2-172">Uppdatera Power BI M-skriptet som du exporterade från Analytics genom att ersätta ARM-URL med AI-API (se exemplet nedan)</span><span class="sxs-lookup"><span data-stu-id="912b2-172">Update the Power BI M script that you exported from Analytics by replacing the ARM URL with AI API (see example below)</span></span>
   * <span data-ttu-id="912b2-173">Ersätt **https://management.azure.com/subscriptions/...**</span><span class="sxs-lookup"><span data-stu-id="912b2-173">Replace **https://management.azure.com/subscriptions/...**</span></span>
   * <span data-ttu-id="912b2-174">med, **https://api.applicationinsights.io/beta/apps/...**</span><span class="sxs-lookup"><span data-stu-id="912b2-174">with, **https://api.applicationinsights.io/beta/apps/...**</span></span>
3. <span data-ttu-id="912b2-175">Slutligen uppdatera autentiseringsuppgifter till grundläggande och Använd API-nyckel</span><span class="sxs-lookup"><span data-stu-id="912b2-175">Finally, update credentials to basic, and use your API Key</span></span>
  

<span data-ttu-id="912b2-176">**Befintligt skript**</span><span class="sxs-lookup"><span data-stu-id="912b2-176">**Existing Script**</span></span>
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
<span data-ttu-id="912b2-177">**Uppdaterat skript**</span><span class="sxs-lookup"><span data-stu-id="912b2-177">**Updated Script**</span></span>
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a><span data-ttu-id="912b2-178">Om provtagning</span><span class="sxs-lookup"><span data-stu-id="912b2-178">About sampling</span></span>
<span data-ttu-id="912b2-179">Om ditt program skickar stora mängder data, kan funktionen för anpassningsbar provtagning fungerar och skicka endast en del av din telemetri.</span><span class="sxs-lookup"><span data-stu-id="912b2-179">If your application sends a lot of data, the adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> <span data-ttu-id="912b2-180">Detsamma gäller om du manuellt har angett samplingsfrekvensen i SDK eller på införandet.</span><span class="sxs-lookup"><span data-stu-id="912b2-180">The same is true if you have manually set sampling either in the SDK or on ingestion.</span></span> [<span data-ttu-id="912b2-181">Läs mer om sampling.</span><span class="sxs-lookup"><span data-stu-id="912b2-181">Learn more about sampling.</span></span>](app-insights-sampling.md)


## <a name="next-steps"></a><span data-ttu-id="912b2-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="912b2-182">Next steps</span></span>
* [<span data-ttu-id="912b2-183">Power BI - Läs</span><span class="sxs-lookup"><span data-stu-id="912b2-183">Power BI - Learn</span></span>](http://www.powerbi.com/learning/)
* [<span data-ttu-id="912b2-184">Analytics-självstudier</span><span class="sxs-lookup"><span data-stu-id="912b2-184">Analytics tutorial</span></span>](app-insights-analytics-tour.md)

