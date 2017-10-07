---
title: "aaaExport med Stream Analytics från Azure Application Insights | Microsoft Docs"
description: "Stream Analytics kan omvandla kontinuerligt, filtrera och väg hello-data som du exporterar från Application Insights."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 31594221-17bd-4e5e-9534-950f3b022209
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: fda9b64f588c520833b2669eafdf650efc3de6be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-stream-analytics-tooprocess-exported-data-from-application-insights"></a><span data-ttu-id="066af-103">Använd Stream Analytics tooprocess exporterade data från Application Insights</span><span class="sxs-lookup"><span data-stu-id="066af-103">Use Stream Analytics tooprocess exported data from Application Insights</span></span>
<span data-ttu-id="066af-104">[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) är hello perfekt verktyg för databearbetning [exporterats från Application Insights](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="066af-104">[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) is hello ideal tool for processing data [exported from Application Insights](app-insights-export-telemetry.md).</span></span> <span data-ttu-id="066af-105">Stream Analytics kan hämta data från olika källor.</span><span class="sxs-lookup"><span data-stu-id="066af-105">Stream Analytics can pull data from a variety of sources.</span></span> <span data-ttu-id="066af-106">Det kan omvandla filtrera hello data och sedan dirigera tooa mängd sänkor.</span><span class="sxs-lookup"><span data-stu-id="066af-106">It can transform and filter hello data, and then route it tooa variety of sinks.</span></span>

<span data-ttu-id="066af-107">I det här exemplet ska vi skapa en adapter som hämtar data från Application Insights, byter namn på och behandlar hello fälten och kommer till Power BI.</span><span class="sxs-lookup"><span data-stu-id="066af-107">In this example, we'll create an adaptor that takes data from Application Insights, renames and processes some of hello fields, and pipes it into Power BI.</span></span>

> [!WARNING]
> <span data-ttu-id="066af-108">Det finns mycket bättre och enklare [rekommenderade sätt toodisplay Application Insights-data i Power BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="066af-108">There are much better and easier [recommended ways toodisplay Application Insights data in Power BI](app-insights-export-power-bi.md).</span></span> <span data-ttu-id="066af-109">hello sökvägen som visas här är bara ett exempel tooillustrate hur tooprocess exporterade data.</span><span class="sxs-lookup"><span data-stu-id="066af-109">hello path illustrated here is just an example tooillustrate how tooprocess exported data.</span></span>
> 
> 

![Blockdiagram för export via SA tooPBI](./media/app-insights-export-stream-analytics/020.png)

## <a name="create-storage-in-azure"></a><span data-ttu-id="066af-111">Skapa lagring i Azure</span><span class="sxs-lookup"><span data-stu-id="066af-111">Create storage in Azure</span></span>
<span data-ttu-id="066af-112">Löpande export alltid matar ut data tooan Azure Storage-konto, så du behöver toocreate hello först.</span><span class="sxs-lookup"><span data-stu-id="066af-112">Continuous export always outputs data tooan Azure Storage account, so you need toocreate hello storage first.</span></span>

1. <span data-ttu-id="066af-113">Skapa ett ”klassisk” storage-konto i din prenumeration i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="066af-113">Create a "classic" storage account in your subscription in hello [Azure portal](https://portal.azure.com).</span></span>
   
   ![Välj ny, Data, lagring i Azure-portalen](./media/app-insights-export-stream-analytics/030.png)
2. <span data-ttu-id="066af-115">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="066af-115">Create a container</span></span>
   
    ![I hello nya lagringsenheter, Välj behållare, klicka på hello behållare panelen och Lägg till](./media/app-insights-export-stream-analytics/040.png)
3. <span data-ttu-id="066af-117">Kopiera hello lagringsåtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="066af-117">Copy hello storage access key</span></span>
   
    <span data-ttu-id="066af-118">Du behöver den snart tooset in hello inkommande toohello stream analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="066af-118">You'll need it soon tooset up hello input toohello stream analytics service.</span></span>
   
    ![Öppna inställningar, nycklar, i hello lagring och göra en kopia av hello primära åtkomstnyckeln](./media/app-insights-export-stream-analytics/045.png)

## <a name="start-continuous-export-tooazure-storage"></a><span data-ttu-id="066af-120">Starta löpande export tooAzure lagring</span><span class="sxs-lookup"><span data-stu-id="066af-120">Start continuous export tooAzure storage</span></span>
<span data-ttu-id="066af-121">[Löpande export](app-insights-export-telemetry.md) flyttar data från Application Insights i Azure storage.</span><span class="sxs-lookup"><span data-stu-id="066af-121">[Continuous export](app-insights-export-telemetry.md) moves data from Application Insights into Azure storage.</span></span>

1. <span data-ttu-id="066af-122">Hello Azure-portalen, bläddra toohello Application Insights-resurs som du har skapat för ditt program.</span><span class="sxs-lookup"><span data-stu-id="066af-122">In hello Azure portal, browse toohello Application Insights resource you created for your application.</span></span>
   
    ![Klicka på Bläddra, Application Insights för ditt program](./media/app-insights-export-stream-analytics/050.png)
2. <span data-ttu-id="066af-124">Skapa en löpande export.</span><span class="sxs-lookup"><span data-stu-id="066af-124">Create a continuous export.</span></span>
   
    ![Välj Lägg till inställningar för löpande Export](./media/app-insights-export-stream-analytics/060.png)

    <span data-ttu-id="066af-126">Välj hello storage-konto som du skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="066af-126">Select hello storage account you created earlier:</span></span>

    ![Ange hello export mål](./media/app-insights-export-stream-analytics/070.png)

    <span data-ttu-id="066af-128">Ange hello händelsetyper som du vill toosee:</span><span class="sxs-lookup"><span data-stu-id="066af-128">Set hello event types you want toosee:</span></span>

    ![Välj händelsetyper](./media/app-insights-export-stream-analytics/080.png)

1. <span data-ttu-id="066af-130">Kan vissa data ackumuleras.</span><span class="sxs-lookup"><span data-stu-id="066af-130">Let some data accumulate.</span></span> <span data-ttu-id="066af-131">Sitta tillbaka och låta personer använder programmet ett tag.</span><span class="sxs-lookup"><span data-stu-id="066af-131">Sit back and let people use your application for a while.</span></span> <span data-ttu-id="066af-132">Telemetri kommer så ser du statistiska diagram i [mått explorer](app-insights-metrics-explorer.md) och enskilda händelser i [diagnostiska Sök](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="066af-132">Telemetry will come in and you'll see statistical charts in [metric explorer](app-insights-metrics-explorer.md) and individual events in [diagnostic search](app-insights-diagnostic-search.md).</span></span> 
   
    <span data-ttu-id="066af-133">Och även hello data kommer att exportera tooyour lagring.</span><span class="sxs-lookup"><span data-stu-id="066af-133">And also, hello data will export tooyour storage.</span></span> 
2. <span data-ttu-id="066af-134">Kontrollera hello exporterade data.</span><span class="sxs-lookup"><span data-stu-id="066af-134">Inspect hello exported data.</span></span> <span data-ttu-id="066af-135">I Visual Studio väljer **visa / Cloud Explorer**, och öppna Azure / lagring.</span><span class="sxs-lookup"><span data-stu-id="066af-135">In Visual Studio, choose **View / Cloud Explorer**, and open Azure / Storage.</span></span> <span data-ttu-id="066af-136">(Om du inte har alternativet, måste tooinstall hello Azure SDK: öppna dialogrutan för hello nytt projekt och Visual C# / molnet / hämta Microsoft Azure SDK för .NET.)</span><span class="sxs-lookup"><span data-stu-id="066af-136">(If you don't have this menu option, you need tooinstall hello Azure SDK: Open hello New Project dialog and open Visual C# / Cloud / Get Microsoft Azure SDK for .NET.)</span></span>
   
    ![](./media/app-insights-export-stream-analytics/04-data.png)
   
    <span data-ttu-id="066af-137">Anteckna hello vanliga tillhör hello sökvägsnamn som härleds från hello namn och instrumentation tangent.</span><span class="sxs-lookup"><span data-stu-id="066af-137">Make a note of hello common part of hello path name, which is derived from hello application name and instrumentation key.</span></span> 

<span data-ttu-id="066af-138">hello-händelser skrivs tooblob filer i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="066af-138">hello events are written tooblob files in JSON format.</span></span> <span data-ttu-id="066af-139">Varje fil kan innehålla en eller flera händelser.</span><span class="sxs-lookup"><span data-stu-id="066af-139">Each file may contain one or more events.</span></span> <span data-ttu-id="066af-140">Så vi vill gärna tooread hello händelsedata och filtrera bort hello fält som vi vill.</span><span class="sxs-lookup"><span data-stu-id="066af-140">So we'd like tooread hello event data and filter out hello fields we want.</span></span> <span data-ttu-id="066af-141">Det finns många olika sätt som vi kan göra med hello data, men vår plan är idag toouse Stream Analytics toopipe hello data tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="066af-141">There are all kinds of things we could do with hello data, but our plan today is toouse Stream Analytics toopipe hello data tooPower BI.</span></span>

## <a name="create-an-azure-stream-analytics-instance"></a><span data-ttu-id="066af-142">Skapa en Azure Stream Analytics-instans</span><span class="sxs-lookup"><span data-stu-id="066af-142">Create an Azure Stream Analytics instance</span></span>
<span data-ttu-id="066af-143">Från hello [klassiska Azure-portalen](https://manage.windowsazure.com/), Välj hello Azure Stream Analytics-tjänsten och skapa ett nytt Stream Analytics-jobb:</span><span class="sxs-lookup"><span data-stu-id="066af-143">From hello [Classic Azure Portal](https://manage.windowsazure.com/), select hello Azure Stream Analytics service, and create a new Stream Analytics job:</span></span>

![](./media/app-insights-export-stream-analytics/090.png)

![](./media/app-insights-export-stream-analytics/100.png)

<span data-ttu-id="066af-144">Expandera information när hello nytt jobb skapas:</span><span class="sxs-lookup"><span data-stu-id="066af-144">When hello new job is created, expand its details:</span></span>

![](./media/app-insights-export-stream-analytics/110.png)

### <a name="set-blob-location"></a><span data-ttu-id="066af-145">Ange blob-plats</span><span class="sxs-lookup"><span data-stu-id="066af-145">Set blob location</span></span>
<span data-ttu-id="066af-146">Ange den tootake indata från din löpande Export blob:</span><span class="sxs-lookup"><span data-stu-id="066af-146">Set it tootake input from your Continuous Export blob:</span></span>

![](./media/app-insights-export-stream-analytics/120.png)

<span data-ttu-id="066af-147">Nu behöver hello primärnyckel åtkomst från ditt Lagringskonto som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="066af-147">Now you'll need hello Primary Access Key from your Storage Account, which you noted earlier.</span></span> <span data-ttu-id="066af-148">Ange som hello Lagringskontonyckel.</span><span class="sxs-lookup"><span data-stu-id="066af-148">Set this as hello Storage Account Key.</span></span>

![](./media/app-insights-export-stream-analytics/130.png)

### <a name="set-path-prefix-pattern"></a><span data-ttu-id="066af-149">Ange sökvägar för prefix</span><span class="sxs-lookup"><span data-stu-id="066af-149">Set path prefix pattern</span></span>
![](./media/app-insights-export-stream-analytics/140.png)

<span data-ttu-id="066af-150">**Vara säker på att tooset hello datumformat tooYYYY-MM-DD (med bindestreck).**</span><span class="sxs-lookup"><span data-stu-id="066af-150">**Be sure tooset hello Date Format tooYYYY-MM-DD (with dashes).**</span></span>

<span data-ttu-id="066af-151">hello prefixet sökvägar anger om Stream Analytics finner hello indatafiler i hello lagring.</span><span class="sxs-lookup"><span data-stu-id="066af-151">hello Path Prefix Pattern specifies where Stream Analytics finds hello input files in hello storage.</span></span> <span data-ttu-id="066af-152">Du behöver tooset den toocorrespond toohow löpande Export lagrar hello data.</span><span class="sxs-lookup"><span data-stu-id="066af-152">You need tooset it toocorrespond toohow Continuous Export stores hello data.</span></span> <span data-ttu-id="066af-153">Ställ in den så här:</span><span class="sxs-lookup"><span data-stu-id="066af-153">Set it like this:</span></span>

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

<span data-ttu-id="066af-154">I det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="066af-154">In this example:</span></span>

* <span data-ttu-id="066af-155">`webapplication27`är hello namnet på hello Application Insights-resurs **alla gemen**.</span><span class="sxs-lookup"><span data-stu-id="066af-155">`webapplication27` is hello name of hello Application Insights resource **all lower case**.</span></span>
* <span data-ttu-id="066af-156">`1234...`är hello instrumentation nyckel för hello Application Insights-resurs **utesluter streck**.</span><span class="sxs-lookup"><span data-stu-id="066af-156">`1234...` is hello instrumentation key of hello Application Insights resource, **omitting dashes**.</span></span> 
* <span data-ttu-id="066af-157">`PageViews`hello typ av data du vill tooanalyze.</span><span class="sxs-lookup"><span data-stu-id="066af-157">`PageViews` is hello type of data you want tooanalyze.</span></span> <span data-ttu-id="066af-158">hello tillgängliga typer är beroende av hello filter som du angett i löpande Export.</span><span class="sxs-lookup"><span data-stu-id="066af-158">hello available types depend on hello filter you set in Continuous Export.</span></span> <span data-ttu-id="066af-159">Granska hello exporterade data toosee hello andra tillgängliga typer och se hello [exportera datamodellen](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="066af-159">Examine hello exported data toosee hello other available types, and see hello [export data model](app-insights-export-data-model.md).</span></span>
* <span data-ttu-id="066af-160">`/{date}/{time}`ett mönster skrivs bokstavligt.</span><span class="sxs-lookup"><span data-stu-id="066af-160">`/{date}/{time}` is a pattern written literally.</span></span>

> [!NOTE]
> <span data-ttu-id="066af-161">Kontrollera hello lagring toomake till att du får hello sökvägen rätt.</span><span class="sxs-lookup"><span data-stu-id="066af-161">Inspect hello storage toomake sure you get hello path right.</span></span>
> 
> 

### <a name="finish-initial-setup"></a><span data-ttu-id="066af-162">Slut installationen</span><span class="sxs-lookup"><span data-stu-id="066af-162">Finish initial setup</span></span>
<span data-ttu-id="066af-163">Bekräfta hello serialiseringsformat:</span><span class="sxs-lookup"><span data-stu-id="066af-163">Confirm hello serialization format:</span></span>

![Bekräfta och Stäng guiden](./media/app-insights-export-stream-analytics/150.png)

<span data-ttu-id="066af-165">Stäng guiden hello och vänta hello installationsprogrammet toocomplete.</span><span class="sxs-lookup"><span data-stu-id="066af-165">Close hello wizard and wait for hello setup toocomplete.</span></span>

> [!TIP]
> <span data-ttu-id="066af-166">Använd hello exempel kommandot toodownload vissa data.</span><span class="sxs-lookup"><span data-stu-id="066af-166">Use hello Sample command toodownload some data.</span></span> <span data-ttu-id="066af-167">Behåll det som ett test exempel toodebug din fråga.</span><span class="sxs-lookup"><span data-stu-id="066af-167">Keep it as a test sample toodebug your query.</span></span>
> 
> 

## <a name="set-hello-output"></a><span data-ttu-id="066af-168">Ange hello utdata</span><span class="sxs-lookup"><span data-stu-id="066af-168">Set hello output</span></span>
<span data-ttu-id="066af-169">Välj ditt jobb och ange hello utdata.</span><span class="sxs-lookup"><span data-stu-id="066af-169">Now select your job and set hello output.</span></span>

![Välj hello ny kanal, klicka på utdata, Lägg till, Power BI](./media/app-insights-export-stream-analytics/160.png)

<span data-ttu-id="066af-171">Ange din **arbets- eller skolkonto** tooauthorize Stream Analytics tooaccess Power BI-resurs.</span><span class="sxs-lookup"><span data-stu-id="066af-171">Provide your **work or school account** tooauthorize Stream Analytics tooaccess your Power BI resource.</span></span> <span data-ttu-id="066af-172">Skriv sedan ett namn för hello utdata och för hello mål Power BI dataset och tabellen.</span><span class="sxs-lookup"><span data-stu-id="066af-172">Then invent a name for hello output, and for hello target Power BI dataset and table.</span></span>

![Tjänsten tre namn](./media/app-insights-export-stream-analytics/170.png)

## <a name="set-hello-query"></a><span data-ttu-id="066af-174">Ange hello fråga</span><span class="sxs-lookup"><span data-stu-id="066af-174">Set hello query</span></span>
<span data-ttu-id="066af-175">hello frågan styr hello översättning av inkommande toooutput.</span><span class="sxs-lookup"><span data-stu-id="066af-175">hello query governs hello translation from input toooutput.</span></span>

![Välj hello jobbet och klicka på frågan.](./media/app-insights-export-stream-analytics/180.png)

<span data-ttu-id="066af-178">Använd hello testa funktionen toocheck att du får rätt hello-utdata.</span><span class="sxs-lookup"><span data-stu-id="066af-178">Use hello Test function toocheck that you get hello right output.</span></span> <span data-ttu-id="066af-179">Ge den hello exempeldata som du tog från hello indata sida.</span><span class="sxs-lookup"><span data-stu-id="066af-179">Give it hello sample data that you took from hello inputs page.</span></span> 

### <a name="query-toodisplay-counts-of-events"></a><span data-ttu-id="066af-180">Frågan toodisplay antal händelser</span><span class="sxs-lookup"><span data-stu-id="066af-180">Query toodisplay counts of events</span></span>
<span data-ttu-id="066af-181">Klistra in den här frågan:</span><span class="sxs-lookup"><span data-stu-id="066af-181">Paste this query:</span></span>

```SQL

    SELECT
      flat.ArrayValue.name,
      count(*)
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.[event]) as flat
    GROUP BY TumblingWindow(minute, 1), flat.ArrayValue.name
```

* <span data-ttu-id="066af-182">export-indata är hello-alias som vi berättade toohello dataström</span><span class="sxs-lookup"><span data-stu-id="066af-182">export-input is hello alias we gave toohello stream input</span></span>
* <span data-ttu-id="066af-183">pbi-utdata är hello kolumnalias vi har definierat</span><span class="sxs-lookup"><span data-stu-id="066af-183">pbi-output is hello output alias we defined</span></span>
* <span data-ttu-id="066af-184">Vi använder [yttre gäller GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) eftersom hello händelsenamnet i en kapslad JSON-arrray.</span><span class="sxs-lookup"><span data-stu-id="066af-184">We use [OUTER APPLY GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) because hello event name is in a nested JSON arrray.</span></span> <span data-ttu-id="066af-185">Sedan hello väljer plockningar hello händelsenamnet tillsammans med antalet hello antal instanser med det namnet i hello tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="066af-185">Then hello Select picks hello event name, together with a count of hello number of instances with that name in hello time period.</span></span> <span data-ttu-id="066af-186">Hej [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) satsen grupper hello element på perioder på 1 minut.</span><span class="sxs-lookup"><span data-stu-id="066af-186">hello [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) clause groups hello elements into time periods of 1 minute.</span></span>

### <a name="query-toodisplay-metric-values"></a><span data-ttu-id="066af-187">Frågan toodisplay måttvärden</span><span class="sxs-lookup"><span data-stu-id="066af-187">Query toodisplay metric values</span></span>
```SQL

    SELECT
      A.context.data.eventtime,
      avg(CASE WHEN flat.arrayvalue.myMetric.value IS NULL THEN 0 ELSE  flat.arrayvalue.myMetric.value END) as myValue
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.context.custom.metrics) as flat
    GROUP BY TumblingWindow(minute, 1), A.context.data.eventtime

``` 

* <span data-ttu-id="066af-188">Den här frågan går hello mått telemetri tooget hello händelsetid och hello värde.</span><span class="sxs-lookup"><span data-stu-id="066af-188">This query drills into hello metrics telemetry tooget hello event time and hello metric value.</span></span> <span data-ttu-id="066af-189">hello måttvärden är i en matris, så vi använder hello yttre gäller GetElements mönster tooextract hello rader.</span><span class="sxs-lookup"><span data-stu-id="066af-189">hello metric values are inside an array, so we use hello OUTER APPLY GetElements pattern tooextract hello rows.</span></span> <span data-ttu-id="066af-190">”myMetric” är hello mått hello namn i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="066af-190">"myMetric" is hello name of hello metric in this case.</span></span> 

### <a name="query-tooinclude-values-of-dimension-properties"></a><span data-ttu-id="066af-191">Frågan tooinclude värdena för dimensionsegenskaper</span><span class="sxs-lookup"><span data-stu-id="066af-191">Query tooinclude values of dimension properties</span></span>
```SQL

    WITH flat AS (
    SELECT
      MySource.context.data.eventTime as eventTime,
      InstanceId = MyDimension.ArrayValue.InstanceId.value,
      BusinessUnitId = MyDimension.ArrayValue.BusinessUnitId.value
    FROM MySource
    OUTER APPLY GetArrayElements(MySource.context.custom.dimensions) MyDimension
    )
    SELECT
     eventTime,
     InstanceId,
     BusinessUnitId
    INTO AIOutput
    FROM flat

```

* <span data-ttu-id="066af-192">Den här frågan innehåller värden för hello dimensionsegenskaper utan beroende på en viss dimension som befinner sig på en fast index i hello dimensionsmatris.</span><span class="sxs-lookup"><span data-stu-id="066af-192">This query includes values of hello dimension properties without depending on a particular dimension being at a fixed index in hello dimension array.</span></span>

## <a name="run-hello-job"></a><span data-ttu-id="066af-193">Kör jobb för hello</span><span class="sxs-lookup"><span data-stu-id="066af-193">Run hello job</span></span>
<span data-ttu-id="066af-194">Du kan välja ett datum i hello tidigare toostart hello jobbet från.</span><span class="sxs-lookup"><span data-stu-id="066af-194">You can select a date in hello past toostart hello job from.</span></span> 

![Välj hello jobbet och klicka på frågan.](./media/app-insights-export-stream-analytics/190.png)

<span data-ttu-id="066af-197">Vänta tills hello jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="066af-197">Wait until hello job is Running.</span></span>

## <a name="see-results-in-power-bi"></a><span data-ttu-id="066af-198">Visa resultatet i Power BI</span><span class="sxs-lookup"><span data-stu-id="066af-198">See results in Power BI</span></span>
> [!WARNING]
> <span data-ttu-id="066af-199">Det finns mycket bättre och enklare [rekommenderade sätt toodisplay Application Insights-data i Power BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="066af-199">There are much better and easier [recommended ways toodisplay Application Insights data in Power BI](app-insights-export-power-bi.md).</span></span> <span data-ttu-id="066af-200">hello sökvägen som visas här är bara ett exempel tooillustrate hur tooprocess exporterade data.</span><span class="sxs-lookup"><span data-stu-id="066af-200">hello path illustrated here is just an example tooillustrate how tooprocess exported data.</span></span>
> 
> 

<span data-ttu-id="066af-201">Öppna Power BI med ditt arbete eller skolans konto och väljer hello dataset och tabellen som du definierade som hello utdata från hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="066af-201">Open Power BI with your work or school account, and select hello dataset and table that you defined as hello output of hello Stream Analytics job.</span></span>

![Välj din datauppsättning och fält i Power BI.](./media/app-insights-export-stream-analytics/200.png)

<span data-ttu-id="066af-203">Nu kan du använda den här datauppsättningen i rapporter och instrumentpaneler i [Power BI](https://powerbi.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="066af-203">Now you can use this dataset in reports and dashboards in [Power BI](https://powerbi.microsoft.com).</span></span>

![Välj din datauppsättning och fält i Power BI.](./media/app-insights-export-stream-analytics/210.png)

## <a name="no-data"></a><span data-ttu-id="066af-205">Ser du inga data?</span><span class="sxs-lookup"><span data-stu-id="066af-205">No data?</span></span>
* <span data-ttu-id="066af-206">Kontrollera att du [set hello datumformat](#set-path-prefix-pattern) korrekt tooYYYY-MM-DD (med bindestreck).</span><span class="sxs-lookup"><span data-stu-id="066af-206">Check that you [set hello date format](#set-path-prefix-pattern) correctly tooYYYY-MM-DD (with dashes).</span></span>

## <a name="video"></a><span data-ttu-id="066af-207">Video</span><span class="sxs-lookup"><span data-stu-id="066af-207">Video</span></span>
<span data-ttu-id="066af-208">Noam Ben Zeev visar hur tooprocess exporteras data med hjälp av Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="066af-208">Noam Ben Zeev shows how tooprocess exported data using Stream Analytics.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Export-to-Power-BI-from-Application-Insights/player]
> 
> 

## <a name="next-steps"></a><span data-ttu-id="066af-209">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="066af-209">Next steps</span></span>
* [<span data-ttu-id="066af-210">Löpande export</span><span class="sxs-lookup"><span data-stu-id="066af-210">Continuous export</span></span>](app-insights-export-telemetry.md)
* [<span data-ttu-id="066af-211">Detaljerad datamodell referens för hello egenskapstyper och värden.</span><span class="sxs-lookup"><span data-stu-id="066af-211">Detailed data model reference for hello property types and values.</span></span>](app-insights-export-data-model.md)
* [<span data-ttu-id="066af-212">Application Insights</span><span class="sxs-lookup"><span data-stu-id="066af-212">Application Insights</span></span>](app-insights-overview.md)

