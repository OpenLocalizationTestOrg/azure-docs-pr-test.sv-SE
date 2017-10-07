---
title: aaaExport logganalys data tooPower BI | Microsoft Docs
description: "Powerbi är en molnbaserad business analytics-tjänst från Microsoft som innehåller omfattande visualiseringar och rapporter för analys av olika datauppsättningar.  Logganalys exportera kontinuerligt data från hello OMS-databasen till Power BI så att du kan utnyttja dess visualiseringar och verktyg för analys.  Den här artikeln beskriver hur tooconfigure frågor i logganalys som exporterar tooPower BI automatiskt med jämna mellanrum."
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
ms.openlocfilehash: 4822f99677e5d1080c72e95cda410da81615bac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="export-log-analytics-data-toopower-bi"></a><span data-ttu-id="988c2-105">Exportera logganalys data tooPower BI</span><span class="sxs-lookup"><span data-stu-id="988c2-105">Export Log Analytics data tooPower BI</span></span>

>[!NOTE]
> <span data-ttu-id="988c2-106">Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), och sedan på den här processen för att exportera logganalys data tooPower BI fungerar inte längre.</span><span class="sxs-lookup"><span data-stu-id="988c2-106">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then this process for exporting Log Analytics data tooPower BI will no longer work.</span></span>  <span data-ttu-id="988c2-107">Alla befintliga scheman som du skapade innan du uppgraderar inaktiveras.</span><span class="sxs-lookup"><span data-stu-id="988c2-107">Any existing schedules that you created before upgrading will become disabled.</span></span> 
>
> <span data-ttu-id="988c2-108">Efter uppgraderingen hello Azure Log Analytics använder samma plattform som Application Insights och du använder samma process tooexport logganalys frågor tooPower BI som hello [hello processen tooexport Application Insights frågar tooPower BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).</span><span class="sxs-lookup"><span data-stu-id="988c2-108">After upgrade, Azure Log Analytics uses hello same platform as Application Insights, and you use hello same process tooexport Log Analytics queries tooPower BI as [hello process tooexport Application Insights queries tooPower BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).</span></span>  <span data-ttu-id="988c2-109">Du kan antingen exportera hello frågan med hello Analytics konsolen enligt beskrivningen i artikeln eller så kan du välja hello **Power BI** knappen hello överst i hello-skärmen i hello loggen Sök-portalen.</span><span class="sxs-lookup"><span data-stu-id="988c2-109">You can either export hello query using hello Analytics console as described in that article, or you can select hello **Power BI** button at hello top of hello screen in hello Log Search portal.</span></span>



<span data-ttu-id="988c2-110">[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) är en molnbaserad business analytics-tjänst från Microsoft som innehåller omfattande visualiseringar och rapporter för analys av olika datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="988c2-110">[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is a cloud based business analytics service from Microsoft that provides rich visualizations and reports for analysis of different sets of data.</span></span>  <span data-ttu-id="988c2-111">Logganalys exportera automatiskt data från hello OMS-databasen till Power BI så att du kan utnyttja dess visualiseringar och verktyg för analys.</span><span class="sxs-lookup"><span data-stu-id="988c2-111">Log Analytics can automatically export data from hello OMS repository into Power BI so you can leverage its visualizations and analysis tools.</span></span>

<span data-ttu-id="988c2-112">När du konfigurerar Power BI med logganalys skapa log-frågor som exporterar deras resultat toocorresponding datauppsättningar i Power BI.</span><span class="sxs-lookup"><span data-stu-id="988c2-112">When you configure Power BI with Log Analytics, you create log queries that export their results toocorresponding datasets in Power BI.</span></span>  <span data-ttu-id="988c2-113">hello frågan och exportera fortsätter tooautomatically körs enligt ett schema som du definierar tookeep hello datamängden in toodate med hello senaste data som samlas in av logganalys.</span><span class="sxs-lookup"><span data-stu-id="988c2-113">hello query and export continues tooautomatically run on a schedule that you define tookeep hello dataset up toodate with hello latest data collected by Log Analytics.</span></span>

![Log Analytics tooPower BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a><span data-ttu-id="988c2-115">Power BI-scheman</span><span class="sxs-lookup"><span data-stu-id="988c2-115">Power BI Schedules</span></span>
<span data-ttu-id="988c2-116">En *Power BI schema* innehåller en logg sökning som exporterar en uppsättning data från hello OMS databasen tooa motsvarande dataset i Power BI och ett schema som definierar hur ofta den här sökningen körs tookeep hello dataset aktuella.</span><span class="sxs-lookup"><span data-stu-id="988c2-116">A *Power BI Schedule* includes a log search that exports a set of data from hello OMS repository tooa corresponding dataset in Power BI and a schedule that defines how often this search is run tookeep hello dataset current.</span></span>

<span data-ttu-id="988c2-117">hello fält i datamängden hello matchar hello egenskaper för hello-poster som returneras av hello loggen sökning.</span><span class="sxs-lookup"><span data-stu-id="988c2-117">hello fields in hello dataset will match hello properties of hello records returned by hello log search.</span></span>  <span data-ttu-id="988c2-118">Om hello sökningen poster med olika typer och sedan hello datauppsättningen tas alla med hello egenskaper från varje hello posttyper.</span><span class="sxs-lookup"><span data-stu-id="988c2-118">If hello search returns records of different types then hello dataset will include all of hello properties from each of hello included record types.</span></span>  

> [!NOTE]
> <span data-ttu-id="988c2-119">Det är en bästa praxis toouse en logg sökfråga som returnerar rådata som skillnad från tooperforming konsolidering med kommandon som [mått](log-analytics-search-reference.md#measure).</span><span class="sxs-lookup"><span data-stu-id="988c2-119">It is a best practice toouse a log search query that returns raw data as opposed tooperforming any consolidation using commands such as [Measure](log-analytics-search-reference.md#measure).</span></span>  <span data-ttu-id="988c2-120">Du kan utföra någon aggregering och beräkningar i Power BI från hello rådata.</span><span class="sxs-lookup"><span data-stu-id="988c2-120">You can perform any aggregation and calculations in Power BI from hello raw data.</span></span>
>
>

## <a name="connecting-oms-workspace-toopower-bi"></a><span data-ttu-id="988c2-121">Ansluta OMS-arbetsytan tooPower BI</span><span class="sxs-lookup"><span data-stu-id="988c2-121">Connecting OMS workspace tooPower BI</span></span>
<span data-ttu-id="988c2-122">Innan du kan exportera från logganalys tooPower BI måste du ansluta din OMS arbetsytan tooyour Power BI-konto med hjälp av hello nedan.</span><span class="sxs-lookup"><span data-stu-id="988c2-122">Before you can export from Log Analytics tooPower BI, you must connect your OMS workspace tooyour Power BI account using hello following procedure.</span></span>  

1. <span data-ttu-id="988c2-123">I hello OMS-konsolen klickar du på hello **inställningar** panelen.</span><span class="sxs-lookup"><span data-stu-id="988c2-123">In hello OMS console click hello **Settings** tile.</span></span>
2. <span data-ttu-id="988c2-124">Välj **konton**.</span><span class="sxs-lookup"><span data-stu-id="988c2-124">Select **Accounts**.</span></span>
3. <span data-ttu-id="988c2-125">I hello **arbetsyteinformation** Klicka på avsnittet **ansluta tooPower BI-konto**.</span><span class="sxs-lookup"><span data-stu-id="988c2-125">In hello **Workspace Information** section click **Connect tooPower BI Account**.</span></span>
4. <span data-ttu-id="988c2-126">Ange hello autentiseringsuppgifter för Power BI-konto.</span><span class="sxs-lookup"><span data-stu-id="988c2-126">Enter hello credentials for your Power BI account.</span></span>

## <a name="create-a-power-bi-schedule"></a><span data-ttu-id="988c2-127">Skapa ett schema för Power BI</span><span class="sxs-lookup"><span data-stu-id="988c2-127">Create a Power BI Schedule</span></span>
<span data-ttu-id="988c2-128">Skapa ett Power BI-schema för varje dataset med hello nedan.</span><span class="sxs-lookup"><span data-stu-id="988c2-128">Create a Power BI Schedule for each dataset using hello following procedure.</span></span>

1. <span data-ttu-id="988c2-129">I hello OMS-konsolen klickar du på hello **loggen Sök** panelen.</span><span class="sxs-lookup"><span data-stu-id="988c2-129">In hello OMS console click hello **Log Search** tile.</span></span>
2. <span data-ttu-id="988c2-130">Ange en ny fråga eller välj en sparad sökning som returnerar hello data som du vill tooexport för**Power BI**.</span><span class="sxs-lookup"><span data-stu-id="988c2-130">Type in a new query or select a saved search that returns hello data that you want tooexport too**Power BI**.</span></span>  
3. <span data-ttu-id="988c2-131">Klicka på hello **Power BI** knappen hello överst i hello sidan tooopen hello **Power BI** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="988c2-131">Click hello **Power BI** button at hello top of hello page tooopen hello **Power BI** dialog.</span></span>
4. <span data-ttu-id="988c2-132">Ange hello information i hello följande tabell och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="988c2-132">Provide hello information in hello following table and click **Save**.</span></span>

| <span data-ttu-id="988c2-133">Egenskap</span><span class="sxs-lookup"><span data-stu-id="988c2-133">Property</span></span> | <span data-ttu-id="988c2-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="988c2-134">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="988c2-135">Namn</span><span class="sxs-lookup"><span data-stu-id="988c2-135">Name</span></span> |<span data-ttu-id="988c2-136">Namnet tooidentify hello schema när du visar hello listan över Power BI-scheman.</span><span class="sxs-lookup"><span data-stu-id="988c2-136">Name tooidentify hello schedule when you view hello list of Power BI schedules.</span></span> |
| <span data-ttu-id="988c2-137">Sparad sökning</span><span class="sxs-lookup"><span data-stu-id="988c2-137">Saved Search</span></span> |<span data-ttu-id="988c2-138">hello loggen Sök toorun.</span><span class="sxs-lookup"><span data-stu-id="988c2-138">hello log search toorun.</span></span>  <span data-ttu-id="988c2-139">Du kan välja hello aktuell fråga, eller så kan du välja en befintlig sparad sökning från hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="988c2-139">You can either select hello current query or select an existing saved search from hello dropdown box.</span></span> |
| <span data-ttu-id="988c2-140">Schema</span><span class="sxs-lookup"><span data-stu-id="988c2-140">Schedule</span></span> |<span data-ttu-id="988c2-141">Hur ofta toorun hello sparade söka och exportera toohello Power BI dataset.</span><span class="sxs-lookup"><span data-stu-id="988c2-141">How often toorun hello saved search and export toohello Power BI dataset.</span></span>  <span data-ttu-id="988c2-142">hello-värdet måste vara mellan 15 minuter och 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="988c2-142">hello value must be between 15 minutes and 24 hours.</span></span> |
| <span data-ttu-id="988c2-143">DataSet-namnet</span><span class="sxs-lookup"><span data-stu-id="988c2-143">Dataset Name</span></span> |<span data-ttu-id="988c2-144">hello namnet på hello dataset i Power BI.</span><span class="sxs-lookup"><span data-stu-id="988c2-144">hello name of hello dataset in Power BI.</span></span>  <span data-ttu-id="988c2-145">Den kommer att skapas om det inte finns och uppdateras om den finns.</span><span class="sxs-lookup"><span data-stu-id="988c2-145">It will be created if it doesn’t exist and updated if it does exist.</span></span> |

## <a name="viewing-and-removing-power-bi-schedules"></a><span data-ttu-id="988c2-146">Visa och ta bort scheman för Power BI</span><span class="sxs-lookup"><span data-stu-id="988c2-146">Viewing and Removing Power BI Schedules</span></span>
<span data-ttu-id="988c2-147">Visa hello en lista över befintliga Power BI-scheman med hello nedan.</span><span class="sxs-lookup"><span data-stu-id="988c2-147">View hello list of existing Power BI Schedules with hello following procedure.</span></span>

1. <span data-ttu-id="988c2-148">I hello OMS-konsolen klickar du på hello **inställningar** panelen.</span><span class="sxs-lookup"><span data-stu-id="988c2-148">In hello OMS console click hello **Settings** tile.</span></span>
2. <span data-ttu-id="988c2-149">Välj **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="988c2-149">Select **Power BI**.</span></span>

<span data-ttu-id="988c2-150">Dessutom toohello information om hello schemalägga, hello antalet gånger som hello schema har körts i hello gångna veckan och hello status för senaste synkronisering för hello visas.</span><span class="sxs-lookup"><span data-stu-id="988c2-150">In addition toohello details of hello schedule, hello number of times that hello schedule has run in hello past week and hello status of hello last sync are displayed.</span></span>  <span data-ttu-id="988c2-151">Om fel uppstod i hello synkronisering klickar du på hello länken toorun en logg söka efter poster med information om hello-fel.</span><span class="sxs-lookup"><span data-stu-id="988c2-151">If hello sync encountered errors, you can click hello link toorun a log search for records with details of hello error.</span></span>

<span data-ttu-id="988c2-152">Du kan ta bort ett schema genom att klicka på hello **X** i hello **ta bort kolumnen**.</span><span class="sxs-lookup"><span data-stu-id="988c2-152">You can remove a schedule by clicking on hello **X** in hello **Remove column**.</span></span>  <span data-ttu-id="988c2-153">Du kan inaktivera ett schema genom att välja **av**.</span><span class="sxs-lookup"><span data-stu-id="988c2-153">You can disable a schedule by selecting **Off**.</span></span>  <span data-ttu-id="988c2-154">toomodify ett schema som du måste ta bort den och återskapa den med nya inställningar för hello.</span><span class="sxs-lookup"><span data-stu-id="988c2-154">toomodify a schedule you must remove it and recreate it with hello new settings.</span></span>

![Power BI-scheman](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a><span data-ttu-id="988c2-156">Exempel genomgång</span><span class="sxs-lookup"><span data-stu-id="988c2-156">Sample walkthrough</span></span>
<span data-ttu-id="988c2-157">hello följande avsnitt beskriver hur ett exempel på att skapa ett schema för Power BI och använda dess dataset toocreate en enkel rapport.</span><span class="sxs-lookup"><span data-stu-id="988c2-157">hello following section walks through an example of creating a Power BI Schedule and using its dataset toocreate a simple report.</span></span>  <span data-ttu-id="988c2-158">I det här exemplet alla prestandadata för en uppsättning datorer är exporterade tooPower BI och sedan ett linjediagram skapas toodisplay processorbelastning.</span><span class="sxs-lookup"><span data-stu-id="988c2-158">In this example, all performance data for a set of computers is exported tooPower BI and then a line graph is created toodisplay processor utilization.</span></span>

### <a name="create-log-search"></a><span data-ttu-id="988c2-159">Skapa loggen sökning</span><span class="sxs-lookup"><span data-stu-id="988c2-159">Create log search</span></span>
<span data-ttu-id="988c2-160">Vi börjar med att skapa en logg sökning efter hello data som vi vill toosend toohello dataset.</span><span class="sxs-lookup"><span data-stu-id="988c2-160">We start by creating a log search for hello data that we want toosend toohello dataset.</span></span>  <span data-ttu-id="988c2-161">I det här exemplet ska vi använda en fråga som returnerar alla prestandadata för datorer med ett namn som börjar med *srv*.</span><span class="sxs-lookup"><span data-stu-id="988c2-161">In this example, we’ll use a query that returns all performance data for computers with a name that starts with *srv*.</span></span>  

![Power BI-scheman](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a><span data-ttu-id="988c2-163">Skapa Power BI-sökning</span><span class="sxs-lookup"><span data-stu-id="988c2-163">Create Power BI Search</span></span>
<span data-ttu-id="988c2-164">Vi klickar på hello **Power BI** knappen tooopen hello Power BI dialogrutan och ange hello krävs information.</span><span class="sxs-lookup"><span data-stu-id="988c2-164">We click hello **Power BI** button tooopen hello Power BI dialog and provide hello required information.</span></span>  <span data-ttu-id="988c2-165">Vi vill att den här sökningen toorun en gång i timmen och skapa en datamängd som kallas *Contoso Perf*.</span><span class="sxs-lookup"><span data-stu-id="988c2-165">We want this search toorun once per hour and create a dataset called *Contoso Perf*.</span></span>  <span data-ttu-id="988c2-166">Eftersom det redan har hello Sök öppna som skapar hello data som vi vill vi behåller hello standardvärdet *Använd aktuell sökfråga* för **sparad sökning**.</span><span class="sxs-lookup"><span data-stu-id="988c2-166">Since we already have hello search open that creates hello data we want, we keep hello default of *Use current search query* for **Saved Search**.</span></span>

![Power BI-sökningen](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a><span data-ttu-id="988c2-168">Kontrollera Power BI-sökningen</span><span class="sxs-lookup"><span data-stu-id="988c2-168">Verify Power BI Search</span></span>
<span data-ttu-id="988c2-169">tooverify att vi har skapat hello schema korrekt, vi visa hello lista över Power BI sökningar under hello **inställningar** panelen i hello OMS-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="988c2-169">tooverify that we created hello schedule correctly, we view hello list of Power BI Searches under hello **Settings** tile in hello OMS dashboard.</span></span>  <span data-ttu-id="988c2-170">Vi Vänta några minuter och uppdatera den här vyn förrän den rapporterar att hello synkronisering har körts.</span><span class="sxs-lookup"><span data-stu-id="988c2-170">We wait several minutes and refresh this view until it reports that hello sync has been run.</span></span>

![Power BI-sökningen](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-hello-dataset-in-power-bi"></a><span data-ttu-id="988c2-172">Verifiera hello datauppsättningen i Power BI</span><span class="sxs-lookup"><span data-stu-id="988c2-172">Verify hello dataset in Power BI</span></span>
<span data-ttu-id="988c2-173">Vi logga in på vår konto på [powerbi.microsoft.com](http://powerbi.microsoft.com/) och rulla för**datauppsättningar** längst hello hello till vänster.</span><span class="sxs-lookup"><span data-stu-id="988c2-173">We log into our account at [powerbi.microsoft.com](http://powerbi.microsoft.com/) and scroll too**Datasets** at hello bottom of hello left pane.</span></span>  <span data-ttu-id="988c2-174">Vi kan se att hello *Contoso Perf* datauppsättningen visas som anger att vår export har kunnat köras.</span><span class="sxs-lookup"><span data-stu-id="988c2-174">We can see that hello *Contoso Perf* dataset is listed indicating that our export has run successfully.</span></span>

![Power BI dataset](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a><span data-ttu-id="988c2-176">Skapa rapport baserad på dataset</span><span class="sxs-lookup"><span data-stu-id="988c2-176">Create report based on dataset</span></span>
<span data-ttu-id="988c2-177">Vi väljer hello **Contoso Perf** dataset och klicka sedan på **resultat** i hello **fält** rutan på hello rätt tooview hello fält som ingår i denna dataset.</span><span class="sxs-lookup"><span data-stu-id="988c2-177">We select hello **Contoso Perf** dataset and then click on **Results** in hello **Fields** pane on hello right tooview hello fields that are part of this dataset.</span></span>  <span data-ttu-id="988c2-178">toocreate en rad diagram som visar processorbelastning för varje dator, vi utföra hello följande åtgärder.</span><span class="sxs-lookup"><span data-stu-id="988c2-178">toocreate a line graph showing processor utilization for each computer, we perform hello following actions.</span></span>

1. <span data-ttu-id="988c2-179">Välj hello rad diagram visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="988c2-179">Select hello Line chart visualization.</span></span>
2. <span data-ttu-id="988c2-180">Dra **ObjectName** för**rapporten nivån filter** och kontrollera **Processor**.</span><span class="sxs-lookup"><span data-stu-id="988c2-180">Drag **ObjectName** too**Report level filter** and check **Processor**.</span></span>
3. <span data-ttu-id="988c2-181">Dra **CounterName** för**rapporten nivån filter** och kontrollera **% processortid**.</span><span class="sxs-lookup"><span data-stu-id="988c2-181">Drag **CounterName** too**Report level filter** and check **% Processor Time**.</span></span>
4. <span data-ttu-id="988c2-182">Dra **CounterValue** för**värden**.</span><span class="sxs-lookup"><span data-stu-id="988c2-182">Drag **CounterValue** too**Values**.</span></span>
5. <span data-ttu-id="988c2-183">Dra **datorn** för**förklaring**.</span><span class="sxs-lookup"><span data-stu-id="988c2-183">Drag **Computer** too**Legend**.</span></span>
6. <span data-ttu-id="988c2-184">Dra **TimeGenerated** för**axel**.</span><span class="sxs-lookup"><span data-stu-id="988c2-184">Drag **TimeGenerated** too**Axis**.</span></span>

<span data-ttu-id="988c2-185">Vi kan se det resulterande linjediagrammet hello visas med hello data från vår datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="988c2-185">We can see that hello resulting line graph is displayed with hello data from our dataset.</span></span>

![Power BI-linjediagram](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-hello-report"></a><span data-ttu-id="988c2-187">Spara hello-rapport</span><span class="sxs-lookup"><span data-stu-id="988c2-187">Save hello report</span></span>
<span data-ttu-id="988c2-188">Vi spara hello rapporten genom att klicka på hello knappen hello överst i hello-skärmen Spara och validera att det visas nu i hello rapporter i hello till vänster.</span><span class="sxs-lookup"><span data-stu-id="988c2-188">We save hello report by clicking on hello Save button at hello top of hello screen and validate that it is now listed in hello Reports section in hello left pane.</span></span>

![Power BI-rapporter](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a><span data-ttu-id="988c2-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="988c2-190">Next steps</span></span>
* <span data-ttu-id="988c2-191">Lär dig mer om [logga sökningar](log-analytics-log-searches.md) toobuild frågor som kan exporteras tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="988c2-191">Learn about [log searches](log-analytics-log-searches.md) toobuild queries that can be exported tooPower BI.</span></span>
* <span data-ttu-id="988c2-192">Lär dig mer om [Power BI](http://powerbi.microsoft.com) toobuild visualiseringar utifrån logganalys exporter.</span><span class="sxs-lookup"><span data-stu-id="988c2-192">Learn more about [Power BI](http://powerbi.microsoft.com) toobuild visualizations based on Log Analytics exports.</span></span>
