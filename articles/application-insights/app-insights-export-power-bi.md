---
title: "aaaExport tooPower BI från Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 6668cd7f4e0fbf41695972617f5f8ec207356659
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="feed-power-bi-from-application-insights"></a><span data-ttu-id="a8eab-103">Powerbi-feed från Application Insights</span><span class="sxs-lookup"><span data-stu-id="a8eab-103">Feed Power BI from Application Insights</span></span>
<span data-ttu-id="a8eab-104">[Power BI](http://www.powerbi.com/) är en uppsättning verktyg för business analytics som hjälper dig att analysera data och dela information.</span><span class="sxs-lookup"><span data-stu-id="a8eab-104">[Power BI](http://www.powerbi.com/) is a suite of business analytics tools that help you analyze data and share insights.</span></span> <span data-ttu-id="a8eab-105">Omfattande instrumentpaneler är tillgängliga på varje enhet.</span><span class="sxs-lookup"><span data-stu-id="a8eab-105">Rich dashboards are available on every device.</span></span> <span data-ttu-id="a8eab-106">Du kan kombinera data från flera källor, inklusive Analytics-frågor från [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a8eab-106">You can combine data from many sources, including Analytics queries from [Azure Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="a8eab-107">Det finns tre rekommenderade metoder för att exportera Application Insights data tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="a8eab-107">There are three recommended methods of exporting Application Insights data tooPower BI.</span></span> <span data-ttu-id="a8eab-108">Du kan använda dem enskilt eller tillsammans.</span><span class="sxs-lookup"><span data-stu-id="a8eab-108">You can use them separately or together.</span></span>

* <span data-ttu-id="a8eab-109">[**Power BI kortet** ](#power-pi-adapter) -Ställ in en komplett instrumentpanel för telemetri från din app.</span><span class="sxs-lookup"><span data-stu-id="a8eab-109">[**Power BI adapter**](#power-pi-adapter) - set up a complete dashboard of telemetry from your app.</span></span> <span data-ttu-id="a8eab-110">hello uppsättning diagram är fördefinierade, men du kan lägga till egna frågor från andra källor.</span><span class="sxs-lookup"><span data-stu-id="a8eab-110">hello set of charts is predefined, but you can add your own queries from any other sources.</span></span>
* <span data-ttu-id="a8eab-111">[**Exportera Analytics frågor** ](#export-analytics-queries) -skriva någon fråga som du vill med hjälp av Analytics och exportera det tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="a8eab-111">[**Export Analytics queries**](#export-analytics-queries) - write any query you want using Analytics, and export it tooPower BI.</span></span> <span data-ttu-id="a8eab-112">Du kan placera den här frågan på en instrumentpanel tillsammans med andra data.</span><span class="sxs-lookup"><span data-stu-id="a8eab-112">You can place this query on a dashboard along with any other data.</span></span>
* <span data-ttu-id="a8eab-113">[**Den löpande exporten och Stream Analytics** ](app-insights-export-stream-analytics.md) -detta innebär mer arbete tooset upp.</span><span class="sxs-lookup"><span data-stu-id="a8eab-113">[**Continuous export and Stream Analytics**](app-insights-export-stream-analytics.md) - This involves more work tooset up.</span></span> <span data-ttu-id="a8eab-114">Det är användbart om du vill tookeep dina data under långa perioder.</span><span class="sxs-lookup"><span data-stu-id="a8eab-114">It is useful if you want tookeep your data for long periods.</span></span> <span data-ttu-id="a8eab-115">Annars rekommenderas hello andra metoder som.</span><span class="sxs-lookup"><span data-stu-id="a8eab-115">Otherwise, hello other methods are recommended.</span></span>

## <a name="power-bi-adapter"></a><span data-ttu-id="a8eab-116">Power BI-kort</span><span class="sxs-lookup"><span data-stu-id="a8eab-116">Power BI adapter</span></span>
<span data-ttu-id="a8eab-117">Den här metoden skapas en komplett instrumentpanel för telemetri.</span><span class="sxs-lookup"><span data-stu-id="a8eab-117">This method creates a complete dashboard of telemetry for you.</span></span> <span data-ttu-id="a8eab-118">hello inledande datauppsättning är fördefinierade, men du kan lägga till mer data tooit.</span><span class="sxs-lookup"><span data-stu-id="a8eab-118">hello initial data set is predefined, but you can add more data tooit.</span></span>

### <a name="get-hello-adapter"></a><span data-ttu-id="a8eab-119">Hämta hello nätverkskort</span><span class="sxs-lookup"><span data-stu-id="a8eab-119">Get hello adapter</span></span>
1. <span data-ttu-id="a8eab-120">Logga in för[Power BI](https://app.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="a8eab-120">Sign in too[Power BI](https://app.powerbi.com/).</span></span>
2. <span data-ttu-id="a8eab-121">Öppna **hämta Data**, **Services**, **Application Insights**</span><span class="sxs-lookup"><span data-stu-id="a8eab-121">Open **Get Data**, **Services**, **Application Insights**</span></span>
   
    ![Hämta från Application Insights-datakälla](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. <span data-ttu-id="a8eab-123">Ange hello information om Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="a8eab-123">Provide hello details of your Application Insights resource.</span></span>
   
    ![Hämta från Application Insights-datakälla](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. <span data-ttu-id="a8eab-125">Vänta en minut eller två för hello data toobe importeras.</span><span class="sxs-lookup"><span data-stu-id="a8eab-125">Wait a minute or two for hello data toobe imported.</span></span>
   
    ![Power BI-kort](./media/app-insights-export-power-bi/010.png)

<span data-ttu-id="a8eab-127">Du kan redigera hello instrumentpanelen kombinera hello Application Insights diagram med de andra källor och med Analytics-frågor.</span><span class="sxs-lookup"><span data-stu-id="a8eab-127">You can edit hello dashboard, combining hello Application Insights charts with those of other sources, and with Analytics queries.</span></span> <span data-ttu-id="a8eab-128">Det finns ett visualiseringen galleri där du kan få fler diagram och varje diagram har en parametrar som du kan ange.</span><span class="sxs-lookup"><span data-stu-id="a8eab-128">There's a visualization gallery where you can get more charts, and each chart has a parameters you can set.</span></span>

<span data-ttu-id="a8eab-129">Efter hello fortsätter importen, hello instrumentpanel och rapporter för hello tooupdate dagligen.</span><span class="sxs-lookup"><span data-stu-id="a8eab-129">After hello initial import, hello dashboard and hello reports continue tooupdate daily.</span></span> <span data-ttu-id="a8eab-130">Du kan styra hello uppdateringsschema för hello datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="a8eab-130">You can control hello refresh schedule on hello dataset.</span></span>

## <a name="export-analytics-queries"></a><span data-ttu-id="a8eab-131">Exportera Analytics-frågor</span><span class="sxs-lookup"><span data-stu-id="a8eab-131">Export Analytics queries</span></span>
<span data-ttu-id="a8eab-132">Den här vägen gör toowrite alla Analytics fråga du och sedan exportera den tooa Power BI-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="a8eab-132">This route allows you toowrite any Analytics query you like, and then export that tooa Power BI dashboard.</span></span> <span data-ttu-id="a8eab-133">(Du kan lägga till toohello instrumentpanel som har skapats av hello-kort).</span><span class="sxs-lookup"><span data-stu-id="a8eab-133">(You can add toohello dashboard created by hello adapter.)</span></span>

### <a name="one-time-install-power-bi-desktop"></a><span data-ttu-id="a8eab-134">En gång: Installera Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="a8eab-134">One time: install Power BI Desktop</span></span>
<span data-ttu-id="a8eab-135">tooimport Application Insights-frågan du använder hello skrivbordsversionen av Power BI.</span><span class="sxs-lookup"><span data-stu-id="a8eab-135">tooimport your Application Insights query, you use hello desktop version of Power BI.</span></span> <span data-ttu-id="a8eab-136">Men sedan kan du publicera den toohello web eller tooyour Power BI molnet arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="a8eab-136">But then you can publish it toohello web or tooyour Power BI cloud workspace.</span></span> 

<span data-ttu-id="a8eab-137">Installera [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).</span><span class="sxs-lookup"><span data-stu-id="a8eab-137">Install [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).</span></span>

### <a name="export-an-analytics-query"></a><span data-ttu-id="a8eab-138">Exportera en Analytics-fråga</span><span class="sxs-lookup"><span data-stu-id="a8eab-138">Export an Analytics query</span></span>
1. <span data-ttu-id="a8eab-139">[Öppna Analytics och skriva en fråga](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="a8eab-139">[Open Analytics and write your query](app-insights-analytics-tour.md).</span></span>
2. <span data-ttu-id="a8eab-140">Testa och finjustera hello frågan tills du är nöjd med resultaten hello.</span><span class="sxs-lookup"><span data-stu-id="a8eab-140">Test and refine hello query until you're happy with hello results.</span></span>

   <span data-ttu-id="a8eab-141">**Kontrollera att den hello-frågan körs korrekt i Analytics innan du exporterar den.**</span><span class="sxs-lookup"><span data-stu-id="a8eab-141">**Make sure that hello query runs correctly in Analytics before you export it.**</span></span>
3. <span data-ttu-id="a8eab-142">På hello **exportera** -menyn, Välj **Power BI (M)**.</span><span class="sxs-lookup"><span data-stu-id="a8eab-142">On hello **Export** menu, choose **Power BI (M)**.</span></span> <span data-ttu-id="a8eab-143">Spara hello textfil.</span><span class="sxs-lookup"><span data-stu-id="a8eab-143">Save hello text file.</span></span>
   
    ![Exportera Power BI-fråga](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. <span data-ttu-id="a8eab-145">I Power BI Desktop select **hämta Data, tom fråga** och sedan i hello-frågan editor under **visa** Välj **avancerade frågeredigeraren**.</span><span class="sxs-lookup"><span data-stu-id="a8eab-145">In Power BI Desktop select **Get Data, Blank Query** and then in hello query editor, under **View** select **Advanced Query Editor**.</span></span>

    <span data-ttu-id="a8eab-146">Klistra in hello exporteras M språk skript i hello avancerade frågeredigeraren.</span><span class="sxs-lookup"><span data-stu-id="a8eab-146">Paste hello exported M Language script into hello Advanced Query Editor.</span></span>

    ![Avancerade frågeredigeraren](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. <span data-ttu-id="a8eab-148">Du kanske tooprovide autentiseringsuppgifter tooallow Power BI tooaccess Azure.</span><span class="sxs-lookup"><span data-stu-id="a8eab-148">You might have tooprovide credentials tooallow Power BI tooaccess Azure.</span></span> <span data-ttu-id="a8eab-149">Använd 'organisationskonto' toosign in med ditt Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="a8eab-149">Use 'organizational account' toosign in with your Microsoft account.</span></span>
   
    ![Ange autentiseringsuppgifter för Azure tooenable Power BI toorun Application Insights-fråga](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    <span data-ttu-id="a8eab-151">(Om du behöver tooverify hello autentiseringsuppgifter kommandot hello inställningar för datakälla-menyn i hello frågeredigeraren.</span><span class="sxs-lookup"><span data-stu-id="a8eab-151">(If you need tooverify hello credentials, use hello Data Source Settings menu command in hello Query Editor.</span></span> <span data-ttu-id="a8eab-152">Var noggrann toospecify hello autentiseringsuppgifter som du använder för Azure, vilket kan skilja sig från dina autentiseringsuppgifter för Power BI.)</span><span class="sxs-lookup"><span data-stu-id="a8eab-152">Take care toospecify hello credentials you use for Azure, which might be different from your credentials for Power BI.)</span></span>
2. <span data-ttu-id="a8eab-153">Välj en visualisering för din fråga och välj hello fält för x-axeln, y-axeln och segmentera dimension.</span><span class="sxs-lookup"><span data-stu-id="a8eab-153">Choose a visualization for your query and select hello fields for x-axis, y-axis, and segmenting dimension.</span></span>
   
    ![Välj visualiseringen](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. <span data-ttu-id="a8eab-155">Publicera din tooyour Power BI molnet rapportarbetsytan.</span><span class="sxs-lookup"><span data-stu-id="a8eab-155">Publish your report tooyour Power BI cloud workspace.</span></span> <span data-ttu-id="a8eab-156">Du kan bädda in en synkroniserade version till andra webbsidor därifrån.</span><span class="sxs-lookup"><span data-stu-id="a8eab-156">From there, you can embed a synchronized version into other web pages.</span></span>
   
    ![Välj visualiseringen](./media/app-insights-export-power-bi/publish-power-bi.png)
4. <span data-ttu-id="a8eab-158">Uppdatera hello rapporten manuellt med intervall, eller ställa in en schemalagd uppdatering på alternativsidan för hello.</span><span class="sxs-lookup"><span data-stu-id="a8eab-158">Refresh hello report manually at intervals, or set up a scheduled refresh on hello options page.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a8eab-159">Felsökning</span><span class="sxs-lookup"><span data-stu-id="a8eab-159">Troubleshooting</span></span>

### <a name="401-or-403-unauthorized"></a><span data-ttu-id="a8eab-160">401 eller 403 obehörig</span><span class="sxs-lookup"><span data-stu-id="a8eab-160">401 or 403 Unauthorized</span></span> 
<span data-ttu-id="a8eab-161">Detta kan inträffa om refesh-token inte har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="a8eab-161">This can happen if your refesh token has not been updated.</span></span> <span data-ttu-id="a8eab-162">Försök dessa steg tooensure fortfarande ha åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a8eab-162">Try these steps tooensure you still have access.</span></span> <span data-ttu-id="a8eab-163">Öppna ett supportärende om du har åtkomst och refershing hello autentiseringsuppgifter fungerar inte.</span><span class="sxs-lookup"><span data-stu-id="a8eab-163">If you do have access and refershing hello credentials does not work, please open a support ticket.</span></span>

1. <span data-ttu-id="a8eab-164">Logga in på hello Azure-portalen och kontrollera att du kan komma åt hello resurs</span><span class="sxs-lookup"><span data-stu-id="a8eab-164">Log into hello Azure Portal and make sure you can access hello resource</span></span>
2. <span data-ttu-id="a8eab-165">Försök toorefresh hello autentiseringsuppgifter för hello instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="a8eab-165">Try toorefresh hello credentials for hello Dashboard</span></span>

### <a name="502-bad-gateway"></a><span data-ttu-id="a8eab-166">502 felaktig Gateway</span><span class="sxs-lookup"><span data-stu-id="a8eab-166">502 Bad Gateway</span></span>
<span data-ttu-id="a8eab-167">Detta orsakas normalt av en Analytics-fråga som returnerar för mycket data.</span><span class="sxs-lookup"><span data-stu-id="a8eab-167">This is usually caused by an Analytics query that returns too much data.</span></span> <span data-ttu-id="a8eab-168">Du bör försöka använda en mindre tidsintervall eller genom att använda hello [sedan](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) eller [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) fungerar endast [projekt](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) hello fält som du behöver.</span><span class="sxs-lookup"><span data-stu-id="a8eab-168">You should try using a smaller time range or by using hello [ago](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) or [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) functions only [project](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) hello fields you need.</span></span>

<span data-ttu-id="a8eab-169">Om att minska hello datauppsättningen kommer från hello Analytics-fråga inte uppfyller dina krav bör du använda hello [API](https://dev.applicationinsights.io/documentation/overview) toopull en större datamängd.</span><span class="sxs-lookup"><span data-stu-id="a8eab-169">If reducing hello dataset coming from hello Analytics query doesn't meet your requirements you should consider using hello [API](https://dev.applicationinsights.io/documentation/overview) toopull a larger dataset.</span></span> <span data-ttu-id="a8eab-170">Här följer instruktioner för hur tooconvert hello M-Query exportera toouse hello API.</span><span class="sxs-lookup"><span data-stu-id="a8eab-170">Here are instructions on how tooconvert hello M-Query export toouse hello API.</span></span>

1. <span data-ttu-id="a8eab-171">Skapa en [API-nyckel](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)</span><span class="sxs-lookup"><span data-stu-id="a8eab-171">Create an [API Key](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)</span></span>
2. <span data-ttu-id="a8eab-172">Uppdatera hello Power BI M-skript som du exporterade från Analytics genom att ersätta hello ARM-URL: en med AI-API (se exemplet nedan)</span><span class="sxs-lookup"><span data-stu-id="a8eab-172">Update hello Power BI M script that you exported from Analytics by replacing hello ARM URL with AI API (see example below)</span></span>
   * <span data-ttu-id="a8eab-173">Ersätt **https://management.azure.com/subscriptions/...**</span><span class="sxs-lookup"><span data-stu-id="a8eab-173">Replace **https://management.azure.com/subscriptions/...**</span></span>
   * <span data-ttu-id="a8eab-174">med, **https://api.applicationinsights.io/beta/apps/...**</span><span class="sxs-lookup"><span data-stu-id="a8eab-174">with, **https://api.applicationinsights.io/beta/apps/...**</span></span>
3. <span data-ttu-id="a8eab-175">Slutligen uppdatera autentiseringsuppgifterna toobasic och Använd API-nyckel</span><span class="sxs-lookup"><span data-stu-id="a8eab-175">Finally, update credentials toobasic, and use your API Key</span></span>
  

<span data-ttu-id="a8eab-176">**Befintligt skript**</span><span class="sxs-lookup"><span data-stu-id="a8eab-176">**Existing Script**</span></span>
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
<span data-ttu-id="a8eab-177">**Uppdaterat skript**</span><span class="sxs-lookup"><span data-stu-id="a8eab-177">**Updated Script**</span></span>
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a><span data-ttu-id="a8eab-178">Om provtagning</span><span class="sxs-lookup"><span data-stu-id="a8eab-178">About sampling</span></span>
<span data-ttu-id="a8eab-179">Om ditt program skickar stora mängder data, hello anpassningsbar provtagning funktionen fungerar och skicka endast en del av din telemetri.</span><span class="sxs-lookup"><span data-stu-id="a8eab-179">If your application sends a lot of data, hello adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> <span data-ttu-id="a8eab-180">Detsamma gäller om du har manuellt provtagning i hello SDK eller på införandet hello.</span><span class="sxs-lookup"><span data-stu-id="a8eab-180">hello same is true if you have manually set sampling either in hello SDK or on ingestion.</span></span> [<span data-ttu-id="a8eab-181">Läs mer om sampling.</span><span class="sxs-lookup"><span data-stu-id="a8eab-181">Learn more about sampling.</span></span>](app-insights-sampling.md)


## <a name="next-steps"></a><span data-ttu-id="a8eab-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8eab-182">Next steps</span></span>
* [<span data-ttu-id="a8eab-183">Power BI - Läs</span><span class="sxs-lookup"><span data-stu-id="a8eab-183">Power BI - Learn</span></span>](http://www.powerbi.com/learning/)
* [<span data-ttu-id="a8eab-184">Analytics-självstudier</span><span class="sxs-lookup"><span data-stu-id="a8eab-184">Analytics tutorial</span></span>](app-insights-analytics-tour.md)

