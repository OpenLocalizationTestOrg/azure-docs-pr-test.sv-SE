---
title: Exportera logganalys data till Power BI | Microsoft Docs
description: "Powerbi är en molnbaserad business analytics-tjänst från Microsoft som innehåller omfattande visualiseringar och rapporter för analys av olika datauppsättningar.  Logganalys exportera kontinuerligt data från OMS-databasen till Power BI så att du kan utnyttja dess visualiseringar och verktyg för analys.  Den här artikeln beskriver hur du konfigurerar frågor i logganalys som exporterar till Power BI automatiskt med jämna mellanrum."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: bwren
ms.openlocfilehash: 98befb16d27387e8f65a27771a2a32c264119d74
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="export-log-analytics-data-to-power-bi"></a><span data-ttu-id="5a016-105">Exportera logganalys data till Power BI</span><span class="sxs-lookup"><span data-stu-id="5a016-105">Export Log Analytics data to Power BI</span></span>

>[!NOTE]
> <span data-ttu-id="5a016-106">Om ditt arbetsområde har uppgraderats till den [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), och sedan på den här processen för att exportera logganalys data till Power BI fungerar inte längre.</span><span class="sxs-lookup"><span data-stu-id="5a016-106">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then this process for exporting Log Analytics data to Power BI will no longer work.</span></span>  <span data-ttu-id="5a016-107">Alla befintliga scheman som du skapade innan du uppgraderar inaktiveras.</span><span class="sxs-lookup"><span data-stu-id="5a016-107">Any existing schedules that you created before upgrading will become disabled.</span></span> 
>
> <span data-ttu-id="5a016-108">Efter uppgraderingen, Azure Log Analytics använder samma plattform som Application Insights och du använder samma process för att exportera logganalys frågor till Power BI som [processen för att exportera Application Insights frågor till Power BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).</span><span class="sxs-lookup"><span data-stu-id="5a016-108">After upgrade, Azure Log Analytics uses the same platform as Application Insights, and you use the same process to export Log Analytics queries to Power BI as [the process to export Application Insights queries to Power BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).</span></span>  <span data-ttu-id="5a016-109">Du kan antingen exportera frågan med hjälp av konsolen Analytics enligt beskrivningen i artikeln eller kan du välja den **Power BI** längst upp på skärmen i loggen Sök-portalen.</span><span class="sxs-lookup"><span data-stu-id="5a016-109">You can either export the query using the Analytics console as described in that article, or you can select the **Power BI** button at the top of the screen in the Log Search portal.</span></span>



<span data-ttu-id="5a016-110">[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) är en molnbaserad business analytics-tjänst från Microsoft som innehåller omfattande visualiseringar och rapporter för analys av olika datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="5a016-110">[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is a cloud based business analytics service from Microsoft that provides rich visualizations and reports for analysis of different sets of data.</span></span>  <span data-ttu-id="5a016-111">Logganalys exportera automatiskt data från OMS-databasen till Power BI så att du kan utnyttja dess visualiseringar och verktyg för analys.</span><span class="sxs-lookup"><span data-stu-id="5a016-111">Log Analytics can automatically export data from the OMS repository into Power BI so you can leverage its visualizations and analysis tools.</span></span>

<span data-ttu-id="5a016-112">När du konfigurerar Power BI med logganalys skapa log-frågor som exporterar resultaten till motsvarande datamängder i Power BI.</span><span class="sxs-lookup"><span data-stu-id="5a016-112">When you configure Power BI with Log Analytics, you create log queries that export their results to corresponding datasets in Power BI.</span></span>  <span data-ttu-id="5a016-113">Frågan och exportera fortsätter automatiskt enligt ett schema som du definierar för att hålla datauppsättningen uppdaterade med den senaste informationen som samlas in av logganalys.</span><span class="sxs-lookup"><span data-stu-id="5a016-113">The query and export continues to automatically run on a schedule that you define to keep the dataset up to date with the latest data collected by Log Analytics.</span></span>

![Logganalys till Powerbi](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a><span data-ttu-id="5a016-115">Power BI-scheman</span><span class="sxs-lookup"><span data-stu-id="5a016-115">Power BI Schedules</span></span>
<span data-ttu-id="5a016-116">En *Power BI schema* innehåller en logg sökning som exporterar en uppsättning data från OMS-databasen till en motsvarande dataset i Power BI och ett schema som definierar hur ofta den här sökningen körs om du vill behålla det aktuella datamängden.</span><span class="sxs-lookup"><span data-stu-id="5a016-116">A *Power BI Schedule* includes a log search that exports a set of data from the OMS repository to a corresponding dataset in Power BI and a schedule that defines how often this search is run to keep the dataset current.</span></span>

<span data-ttu-id="5a016-117">Fält i datamängden matchar egenskaperna för de poster som returneras av loggen sökningen.</span><span class="sxs-lookup"><span data-stu-id="5a016-117">The fields in the dataset will match the properties of the records returned by the log search.</span></span>  <span data-ttu-id="5a016-118">Om sökningen innehåller olika typer av sedan tas datamängden alla egenskaper från var och en av typerna av inkluderade poster.</span><span class="sxs-lookup"><span data-stu-id="5a016-118">If the search returns records of different types then the dataset will include all of the properties from each of the included record types.</span></span>  

> [!NOTE]
> <span data-ttu-id="5a016-119">Det är en bra idé att använda en logg sökfråga som returnerar rådata och utföra konsolidering med kommandon som [mått](log-analytics-search-reference.md#measure).</span><span class="sxs-lookup"><span data-stu-id="5a016-119">It is a best practice to use a log search query that returns raw data as opposed to performing any consolidation using commands such as [Measure](log-analytics-search-reference.md#measure).</span></span>  <span data-ttu-id="5a016-120">Du kan utföra någon aggregering och beräkningar i Power BI från rådata.</span><span class="sxs-lookup"><span data-stu-id="5a016-120">You can perform any aggregation and calculations in Power BI from the raw data.</span></span>
>
>

## <a name="connecting-oms-workspace-to-power-bi"></a><span data-ttu-id="5a016-121">Ansluter OMS-arbetsytan till Power BI</span><span class="sxs-lookup"><span data-stu-id="5a016-121">Connecting OMS workspace to Power BI</span></span>
<span data-ttu-id="5a016-122">Innan du kan exportera från logganalys till Power BI måste du ansluta din OMS-arbetsyta till ditt Power BI-konto med följande procedur.</span><span class="sxs-lookup"><span data-stu-id="5a016-122">Before you can export from Log Analytics to Power BI, you must connect your OMS workspace to your Power BI account using the following procedure.</span></span>  

1. <span data-ttu-id="5a016-123">I OMS-konsolen klickar du på den **inställningar** panelen.</span><span class="sxs-lookup"><span data-stu-id="5a016-123">In the OMS console click the **Settings** tile.</span></span>
2. <span data-ttu-id="5a016-124">Välj **konton**.</span><span class="sxs-lookup"><span data-stu-id="5a016-124">Select **Accounts**.</span></span>
3. <span data-ttu-id="5a016-125">I den **arbetsyteinformation** Klicka på avsnittet **Anslut till Power BI-konto**.</span><span class="sxs-lookup"><span data-stu-id="5a016-125">In the **Workspace Information** section click **Connect to Power BI Account**.</span></span>
4. <span data-ttu-id="5a016-126">Ange autentiseringsuppgifterna för ditt Power BI-konto.</span><span class="sxs-lookup"><span data-stu-id="5a016-126">Enter the credentials for your Power BI account.</span></span>

## <a name="create-a-power-bi-schedule"></a><span data-ttu-id="5a016-127">Skapa ett schema för Power BI</span><span class="sxs-lookup"><span data-stu-id="5a016-127">Create a Power BI Schedule</span></span>
<span data-ttu-id="5a016-128">Skapa ett Power BI-schema för varje dataset med följande procedur.</span><span class="sxs-lookup"><span data-stu-id="5a016-128">Create a Power BI Schedule for each dataset using the following procedure.</span></span>

1. <span data-ttu-id="5a016-129">I OMS-konsolen klickar du på den **loggen Sök** panelen.</span><span class="sxs-lookup"><span data-stu-id="5a016-129">In the OMS console click the **Log Search** tile.</span></span>
2. <span data-ttu-id="5a016-130">Ange en ny fråga eller välj en sparad sökning som returnerar de data som du vill exportera till **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="5a016-130">Type in a new query or select a saved search that returns the data that you want to export to **Power BI**.</span></span>  
3. <span data-ttu-id="5a016-131">Klicka på den **Power BI** längst upp på sidan för att öppna den **Power BI** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5a016-131">Click the **Power BI** button at the top of the page to open the **Power BI** dialog.</span></span>
4. <span data-ttu-id="5a016-132">Ange information i följande tabell och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="5a016-132">Provide the information in the following table and click **Save**.</span></span>

| <span data-ttu-id="5a016-133">Egenskap</span><span class="sxs-lookup"><span data-stu-id="5a016-133">Property</span></span> | <span data-ttu-id="5a016-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5a016-134">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="5a016-135">Namn</span><span class="sxs-lookup"><span data-stu-id="5a016-135">Name</span></span> |<span data-ttu-id="5a016-136">Namn för att identifiera schemat när du visar listan över Power BI-scheman.</span><span class="sxs-lookup"><span data-stu-id="5a016-136">Name to identify the schedule when you view the list of Power BI schedules.</span></span> |
| <span data-ttu-id="5a016-137">Sparad sökning</span><span class="sxs-lookup"><span data-stu-id="5a016-137">Saved Search</span></span> |<span data-ttu-id="5a016-138">Loggen genomsökas.</span><span class="sxs-lookup"><span data-stu-id="5a016-138">The log search to run.</span></span>  <span data-ttu-id="5a016-139">Du kan välja den aktuella frågan eller välj en befintlig sparad sökning i listrutan.</span><span class="sxs-lookup"><span data-stu-id="5a016-139">You can either select the current query or select an existing saved search from the dropdown box.</span></span> |
| <span data-ttu-id="5a016-140">Schema</span><span class="sxs-lookup"><span data-stu-id="5a016-140">Schedule</span></span> |<span data-ttu-id="5a016-141">Hur ofta du kör den sparade sökningen och exportera till Power BI-datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="5a016-141">How often to run the saved search and export to the Power BI dataset.</span></span>  <span data-ttu-id="5a016-142">Värdet måste vara mellan 15 minuter och 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="5a016-142">The value must be between 15 minutes and 24 hours.</span></span> |
| <span data-ttu-id="5a016-143">DataSet-namnet</span><span class="sxs-lookup"><span data-stu-id="5a016-143">Dataset Name</span></span> |<span data-ttu-id="5a016-144">Namnet på dataset i Power BI.</span><span class="sxs-lookup"><span data-stu-id="5a016-144">The name of the dataset in Power BI.</span></span>  <span data-ttu-id="5a016-145">Den kommer att skapas om det inte finns och uppdateras om den finns.</span><span class="sxs-lookup"><span data-stu-id="5a016-145">It will be created if it doesn’t exist and updated if it does exist.</span></span> |

## <a name="viewing-and-removing-power-bi-schedules"></a><span data-ttu-id="5a016-146">Visa och ta bort scheman för Power BI</span><span class="sxs-lookup"><span data-stu-id="5a016-146">Viewing and Removing Power BI Schedules</span></span>
<span data-ttu-id="5a016-147">Visa listan över befintliga Power BI-scheman med följande procedur.</span><span class="sxs-lookup"><span data-stu-id="5a016-147">View the list of existing Power BI Schedules with the following procedure.</span></span>

1. <span data-ttu-id="5a016-148">I OMS-konsolen klickar du på den **inställningar** panelen.</span><span class="sxs-lookup"><span data-stu-id="5a016-148">In the OMS console click the **Settings** tile.</span></span>
2. <span data-ttu-id="5a016-149">Välj **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="5a016-149">Select **Power BI**.</span></span>

<span data-ttu-id="5a016-150">Förutom information om schemat visas status för den senaste synkroniseringen och antalet gånger som schemat har körts under den senaste veckan.</span><span class="sxs-lookup"><span data-stu-id="5a016-150">In addition to the details of the schedule, the number of times that the schedule has run in the past week and the status of the last sync are displayed.</span></span>  <span data-ttu-id="5a016-151">Om fel uppstod i synkroniseringen, klickar du på länken om du vill köra en logg Sök efter poster med information om felet.</span><span class="sxs-lookup"><span data-stu-id="5a016-151">If the sync encountered errors, you can click the link to run a log search for records with details of the error.</span></span>

<span data-ttu-id="5a016-152">Du kan ta bort ett schema genom att klicka på den **X** i den **ta bort kolumnen**.</span><span class="sxs-lookup"><span data-stu-id="5a016-152">You can remove a schedule by clicking on the **X** in the **Remove column**.</span></span>  <span data-ttu-id="5a016-153">Du kan inaktivera ett schema genom att välja **av**.</span><span class="sxs-lookup"><span data-stu-id="5a016-153">You can disable a schedule by selecting **Off**.</span></span>  <span data-ttu-id="5a016-154">Om du vill ändra ett schema måste du ta bort den och återskapa den med de nya inställningarna.</span><span class="sxs-lookup"><span data-stu-id="5a016-154">To modify a schedule you must remove it and recreate it with the new settings.</span></span>

![Power BI-scheman](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a><span data-ttu-id="5a016-156">Exempel genomgång</span><span class="sxs-lookup"><span data-stu-id="5a016-156">Sample walkthrough</span></span>
<span data-ttu-id="5a016-157">Följande avsnitt beskriver hur ett exempel för att skapa ett schema för Power BI och använda dess dataset för att skapa en enkel rapport.</span><span class="sxs-lookup"><span data-stu-id="5a016-157">The following section walks through an example of creating a Power BI Schedule and using its dataset to create a simple report.</span></span>  <span data-ttu-id="5a016-158">I det här exemplet alla prestandadata för en uppsättning datorer som har exporterats till Power BI och sedan ett linjediagram skapas om du vill visa processorbelastning.</span><span class="sxs-lookup"><span data-stu-id="5a016-158">In this example, all performance data for a set of computers is exported to Power BI and then a line graph is created to display processor utilization.</span></span>

### <a name="create-log-search"></a><span data-ttu-id="5a016-159">Skapa loggen sökning</span><span class="sxs-lookup"><span data-stu-id="5a016-159">Create log search</span></span>
<span data-ttu-id="5a016-160">Vi börjar med att skapa en logg Sök efter de data som vi vill skicka till dataset.</span><span class="sxs-lookup"><span data-stu-id="5a016-160">We start by creating a log search for the data that we want to send to the dataset.</span></span>  <span data-ttu-id="5a016-161">I det här exemplet ska vi använda en fråga som returnerar alla prestandadata för datorer med ett namn som börjar med *srv*.</span><span class="sxs-lookup"><span data-stu-id="5a016-161">In this example, we’ll use a query that returns all performance data for computers with a name that starts with *srv*.</span></span>  

![Power BI-scheman](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a><span data-ttu-id="5a016-163">Skapa Power BI-sökning</span><span class="sxs-lookup"><span data-stu-id="5a016-163">Create Power BI Search</span></span>
<span data-ttu-id="5a016-164">Vi klickar på den **Power BI** knappen för att öppna dialogrutan Power BI och ange informationen som krävs.</span><span class="sxs-lookup"><span data-stu-id="5a016-164">We click the **Power BI** button to open the Power BI dialog and provide the required information.</span></span>  <span data-ttu-id="5a016-165">Vi vill sökningen att köra en gång i timmen och skapa en datamängd som kallas *Contoso Perf*.</span><span class="sxs-lookup"><span data-stu-id="5a016-165">We want this search to run once per hour and create a dataset called *Contoso Perf*.</span></span>  <span data-ttu-id="5a016-166">Eftersom det redan har Sök öppna som skapar de data som vi vill vi behåller du standardvärdet *Använd aktuell sökfråga* för **sparad sökning**.</span><span class="sxs-lookup"><span data-stu-id="5a016-166">Since we already have the search open that creates the data we want, we keep the default of *Use current search query* for **Saved Search**.</span></span>

![Power BI-sökningen](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a><span data-ttu-id="5a016-168">Kontrollera Power BI-sökningen</span><span class="sxs-lookup"><span data-stu-id="5a016-168">Verify Power BI Search</span></span>
<span data-ttu-id="5a016-169">Kontrollera att vi skapade schemat korrekt, vi visa listan över Power BI-sökningar under den **inställningar** panelen i OMS-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="5a016-169">To verify that we created the schedule correctly, we view the list of Power BI Searches under the **Settings** tile in the OMS dashboard.</span></span>  <span data-ttu-id="5a016-170">Vi Vänta några minuter och uppdatera den här vyn förrän den rapporterar att synkroniseringen har körts.</span><span class="sxs-lookup"><span data-stu-id="5a016-170">We wait several minutes and refresh this view until it reports that the sync has been run.</span></span>

![Power BI-sökningen](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a><span data-ttu-id="5a016-172">Verifiera datauppsättningen i Power BI</span><span class="sxs-lookup"><span data-stu-id="5a016-172">Verify the dataset in Power BI</span></span>
<span data-ttu-id="5a016-173">Vi logga in på vår konto på [powerbi.microsoft.com](http://powerbi.microsoft.com/) och bläddrar till **datauppsättningar** längst ned i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="5a016-173">We log into our account at [powerbi.microsoft.com](http://powerbi.microsoft.com/) and scroll to **Datasets** at the bottom of the left pane.</span></span>  <span data-ttu-id="5a016-174">Vi kan se att den *Contoso Perf* datauppsättningen visas som anger att vår export har kunnat köras.</span><span class="sxs-lookup"><span data-stu-id="5a016-174">We can see that the *Contoso Perf* dataset is listed indicating that our export has run successfully.</span></span>

![Power BI dataset](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a><span data-ttu-id="5a016-176">Skapa rapport baserad på dataset</span><span class="sxs-lookup"><span data-stu-id="5a016-176">Create report based on dataset</span></span>
<span data-ttu-id="5a016-177">Vi väljer den **Contoso Perf** dataset och klicka sedan på **resultat** i den **fält** rutan till höger för att visa vilka fält som är en del av denna dataset.</span><span class="sxs-lookup"><span data-stu-id="5a016-177">We select the **Contoso Perf** dataset and then click on **Results** in the **Fields** pane on the right to view the fields that are part of this dataset.</span></span>  <span data-ttu-id="5a016-178">Vi utföra följande åtgärder för att skapa ett linjediagram visar processorbelastning för varje dator.</span><span class="sxs-lookup"><span data-stu-id="5a016-178">To create a line graph showing processor utilization for each computer, we perform the following actions.</span></span>

1. <span data-ttu-id="5a016-179">Välj diagramvisualisering som rad.</span><span class="sxs-lookup"><span data-stu-id="5a016-179">Select the Line chart visualization.</span></span>
2. <span data-ttu-id="5a016-180">Dra **ObjectName** till **rapporten nivån filter** och kontrollera **Processor**.</span><span class="sxs-lookup"><span data-stu-id="5a016-180">Drag **ObjectName** to **Report level filter** and check **Processor**.</span></span>
3. <span data-ttu-id="5a016-181">Dra **CounterName** till **rapporten nivån filter** och kontrollera **% processortid**.</span><span class="sxs-lookup"><span data-stu-id="5a016-181">Drag **CounterName** to **Report level filter** and check **% Processor Time**.</span></span>
4. <span data-ttu-id="5a016-182">Dra **CounterValue** till **värden**.</span><span class="sxs-lookup"><span data-stu-id="5a016-182">Drag **CounterValue** to **Values**.</span></span>
5. <span data-ttu-id="5a016-183">Dra **datorn** till **förklaring**.</span><span class="sxs-lookup"><span data-stu-id="5a016-183">Drag **Computer** to **Legend**.</span></span>
6. <span data-ttu-id="5a016-184">Dra **TimeGenerated** till **axel**.</span><span class="sxs-lookup"><span data-stu-id="5a016-184">Drag **TimeGenerated** to **Axis**.</span></span>

<span data-ttu-id="5a016-185">Vi kan se att det resulterande linjediagrammet visas med data från vår datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="5a016-185">We can see that the resulting line graph is displayed with the data from our dataset.</span></span>

![Power BI-linjediagram](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a><span data-ttu-id="5a016-187">Spara rapporten</span><span class="sxs-lookup"><span data-stu-id="5a016-187">Save the report</span></span>
<span data-ttu-id="5a016-188">Vi sparar rapporten genom att klicka på knappen Spara längst upp på skärmen och verifiera att det visas nu i området rapporter i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="5a016-188">We save the report by clicking on the Save button at the top of the screen and validate that it is now listed in the Reports section in the left pane.</span></span>

![Power BI-rapporter](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a><span data-ttu-id="5a016-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5a016-190">Next steps</span></span>
* <span data-ttu-id="5a016-191">Lär dig mer om [logga sökningar](log-analytics-log-searches.md) att skapa frågor som kan exporteras till Power BI.</span><span class="sxs-lookup"><span data-stu-id="5a016-191">Learn about [log searches](log-analytics-log-searches.md) to build queries that can be exported to Power BI.</span></span>
* <span data-ttu-id="5a016-192">Lär dig mer om [Power BI](http://powerbi.microsoft.com) att skapa grafik baserad på logganalys exporter.</span><span class="sxs-lookup"><span data-stu-id="5a016-192">Learn more about [Power BI](http://powerbi.microsoft.com) to build visualizations based on Log Analytics exports.</span></span>
