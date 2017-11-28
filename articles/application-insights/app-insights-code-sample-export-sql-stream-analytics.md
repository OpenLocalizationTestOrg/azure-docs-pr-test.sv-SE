---
title: "Exportera tooSQL från Azure Application Insights | Microsoft Docs"
description: Exportera kontinuerligt Application Insights data tooSQL med Stream Analytics.
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 48903032-2c99-4987-9948-d6e4559b4a63
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/06/2015
ms.author: bwren
ms.openlocfilehash: 58b579499113751a088dc7e66cbec71529773322
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-export-toosql-from-application-insights-using-stream-analytics"></a><span data-ttu-id="7dc25-103">Genomgång: Exportera tooSQL från Application Insights med Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="7dc25-103">Walkthrough: Export tooSQL from Application Insights using Stream Analytics</span></span>
<span data-ttu-id="7dc25-104">Den här artikeln visar hur toomove telemetridata från [Azure Application Insights] [ start] till en Azure SQL-databas med hjälp av [löpande Export] [ export] och [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="7dc25-104">This article shows how toomove your telemetry data from [Azure Application Insights][start] into an Azure SQL database by using [Continuous Export][export] and [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span></span> 

<span data-ttu-id="7dc25-105">Löpande export flyttar telemetridata till Azure Storage i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="7dc25-105">Continuous export moves your telemetry data into Azure Storage in JSON format.</span></span> <span data-ttu-id="7dc25-106">Vi parsa hello JSON-objekt med Azure Stream Analytics och skapa rader i en databastabell.</span><span class="sxs-lookup"><span data-stu-id="7dc25-106">We'll parse hello JSON objects using Azure Stream Analytics and create rows in a database table.</span></span>

<span data-ttu-id="7dc25-107">(Mer generellt löpande Export har hello sätt toodo analyser om hello telemetri dina appar skicka tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="7dc25-107">(More generally, Continuous Export is hello way toodo your own analysis of hello telemetry your apps send tooApplication Insights.</span></span> <span data-ttu-id="7dc25-108">Du kan anpassa den här koden exempel toodo andra saker med hello exporteras telemetri, till exempel aggregering av data.)</span><span class="sxs-lookup"><span data-stu-id="7dc25-108">You could adapt this code sample toodo other things with hello exported telemetry, such as aggregation of data.)</span></span>

<span data-ttu-id="7dc25-109">Vi börjar med hello antagandet att du redan har hello-app som du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="7dc25-109">We'll start with hello assumption that you already have hello app you want toomonitor.</span></span>

<span data-ttu-id="7dc25-110">I det här exemplet ska vi använda hello data om sidvisningar, men hello samma mönster kan enkelt utökas tooother datatyper, till exempel anpassade händelser och undantag.</span><span class="sxs-lookup"><span data-stu-id="7dc25-110">In this example, we will be using hello page view data, but hello same pattern can easily be extended tooother data types such as custom events and exceptions.</span></span> 

## <a name="add-application-insights-tooyour-application"></a><span data-ttu-id="7dc25-111">Lägg till Application Insights tooyour program</span><span class="sxs-lookup"><span data-stu-id="7dc25-111">Add Application Insights tooyour application</span></span>
<span data-ttu-id="7dc25-112">tooget igång:</span><span class="sxs-lookup"><span data-stu-id="7dc25-112">tooget started:</span></span>

1. <span data-ttu-id="7dc25-113">[Konfigurera Application Insights för webbsidor](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="7dc25-113">[Set up Application Insights for your web pages](app-insights-javascript.md).</span></span> 
   
    <span data-ttu-id="7dc25-114">(I det här exemplet ska vi fokusera på bearbetning av data om sidvisningar från hello klientwebbläsare, men du kan också ställa in Programinsikter för hello serversidan av din [Java](app-insights-java-get-started.md) eller [ASP.NET](app-insights-asp-net.md) appen och processen för begäran beroende- och andra server telemetri.)</span><span class="sxs-lookup"><span data-stu-id="7dc25-114">(In this example, we'll focus on processing page view data from hello client browsers, but you could also set up Application Insights for hello server side of your [Java](app-insights-java-get-started.md) or [ASP.NET](app-insights-asp-net.md) app and process request, dependency and other server telemetry.)</span></span>
2. <span data-ttu-id="7dc25-115">Publicera din app och titta på telemetridata som visas i Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="7dc25-115">Publish your app, and watch telemetry data appearing in your Application Insights resource.</span></span>

## <a name="create-storage-in-azure"></a><span data-ttu-id="7dc25-116">Skapa lagring i Azure</span><span class="sxs-lookup"><span data-stu-id="7dc25-116">Create storage in Azure</span></span>
<span data-ttu-id="7dc25-117">Löpande export alltid matar ut data tooan Azure Storage-konto, så du behöver toocreate hello först.</span><span class="sxs-lookup"><span data-stu-id="7dc25-117">Continuous export always outputs data tooan Azure Storage account, so you need toocreate hello storage first.</span></span>

1. <span data-ttu-id="7dc25-118">Skapa ett lagringskonto i din prenumeration i hello [Azure-portalen][portal].</span><span class="sxs-lookup"><span data-stu-id="7dc25-118">Create a storage account in your subscription in hello [Azure portal][portal].</span></span>
   
    ![Välj ny, Data, lagring i Azure-portalen.](./media/app-insights-code-sample-export-sql-stream-analytics/040-store.png)
2. <span data-ttu-id="7dc25-122">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="7dc25-122">Create a container</span></span>
   
    ![I hello nya lagringsenheter, Välj behållare, klicka på hello behållare panelen och Lägg till](./media/app-insights-code-sample-export-sql-stream-analytics/050-container.png)
3. <span data-ttu-id="7dc25-124">Kopiera hello lagringsåtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="7dc25-124">Copy hello storage access key</span></span>
   
    <span data-ttu-id="7dc25-125">Du behöver den snart tooset in hello inkommande toohello stream analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7dc25-125">You'll need it soon tooset up hello input toohello stream analytics service.</span></span>
   
    ![Öppna inställningar, nycklar, i hello lagring och göra en kopia av hello primära åtkomstnyckeln](./media/app-insights-code-sample-export-sql-stream-analytics/21-storage-key.png)

## <a name="start-continuous-export-tooazure-storage"></a><span data-ttu-id="7dc25-127">Starta löpande export tooAzure lagring</span><span class="sxs-lookup"><span data-stu-id="7dc25-127">Start continuous export tooAzure storage</span></span>
1. <span data-ttu-id="7dc25-128">Hello Azure-portalen, bläddra toohello Application Insights-resurs som du har skapat för ditt program.</span><span class="sxs-lookup"><span data-stu-id="7dc25-128">In hello Azure portal, browse toohello Application Insights resource you created for your application.</span></span>
   
    ![Klicka på Bläddra, Application Insights för ditt program](./media/app-insights-code-sample-export-sql-stream-analytics/060-browse.png)
2. <span data-ttu-id="7dc25-130">Skapa en löpande export.</span><span class="sxs-lookup"><span data-stu-id="7dc25-130">Create a continuous export.</span></span>
   
    ![Välj Lägg till inställningar för löpande Export](./media/app-insights-code-sample-export-sql-stream-analytics/070-export.png)

    <span data-ttu-id="7dc25-132">Välj hello storage-konto som du skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="7dc25-132">Select hello storage account you created earlier:</span></span>

    ![Ange hello export mål](./media/app-insights-code-sample-export-sql-stream-analytics/080-add.png)

    <span data-ttu-id="7dc25-134">Ange hello händelsetyper som du vill toosee:</span><span class="sxs-lookup"><span data-stu-id="7dc25-134">Set hello event types you want toosee:</span></span>

    ![Välj händelsetyper](./media/app-insights-code-sample-export-sql-stream-analytics/085-types.png)


1. <span data-ttu-id="7dc25-136">Kan vissa data ackumuleras.</span><span class="sxs-lookup"><span data-stu-id="7dc25-136">Let some data accumulate.</span></span> <span data-ttu-id="7dc25-137">Sitta tillbaka och låta personer använder programmet ett tag.</span><span class="sxs-lookup"><span data-stu-id="7dc25-137">Sit back and let people use your application for a while.</span></span> <span data-ttu-id="7dc25-138">Telemetri kommer så ser du statistiska diagram i [mått explorer](app-insights-metrics-explorer.md) och enskilda händelser i [diagnostiska Sök](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="7dc25-138">Telemetry will come in and you'll see statistical charts in [metric explorer](app-insights-metrics-explorer.md) and individual events in [diagnostic search](app-insights-diagnostic-search.md).</span></span> 
   
    <span data-ttu-id="7dc25-139">Och även hello data kommer att exportera tooyour lagring.</span><span class="sxs-lookup"><span data-stu-id="7dc25-139">And also, hello data will export tooyour storage.</span></span> 
2. <span data-ttu-id="7dc25-140">Inspektera hello exporterade data, antingen i hello portal - Välj **Bläddra**, Välj ditt lagringskonto och sedan **behållare** - eller i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7dc25-140">Inspect hello exported data, either in hello portal - choose **Browse**, select your storage account, and then **Containers** - or in Visual Studio.</span></span> <span data-ttu-id="7dc25-141">I Visual Studio väljer **visa / Cloud Explorer**, och öppna Azure / lagring.</span><span class="sxs-lookup"><span data-stu-id="7dc25-141">In Visual Studio, choose **View / Cloud Explorer**, and open Azure / Storage.</span></span> <span data-ttu-id="7dc25-142">(Om du inte har alternativet, måste tooinstall hello Azure SDK: öppna dialogrutan för hello nytt projekt och Visual C# / molnet / hämta Microsoft Azure SDK för .NET.)</span><span class="sxs-lookup"><span data-stu-id="7dc25-142">(If you don't have this menu option, you need tooinstall hello Azure SDK: Open hello New Project dialog and open Visual C# / Cloud / Get Microsoft Azure SDK for .NET.)</span></span>
   
    ![Öppna i Visual Studio, servrar, Azure, lagring](./media/app-insights-code-sample-export-sql-stream-analytics/087-explorer.png)
   
    <span data-ttu-id="7dc25-144">Anteckna hello vanliga tillhör hello sökvägsnamn som härleds från hello namn och instrumentation tangent.</span><span class="sxs-lookup"><span data-stu-id="7dc25-144">Make a note of hello common part of hello path name, which is derived from hello application name and instrumentation key.</span></span> 

<span data-ttu-id="7dc25-145">hello-händelser skrivs tooblob filer i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="7dc25-145">hello events are written tooblob files in JSON format.</span></span> <span data-ttu-id="7dc25-146">Varje fil kan innehålla en eller flera händelser.</span><span class="sxs-lookup"><span data-stu-id="7dc25-146">Each file may contain one or more events.</span></span> <span data-ttu-id="7dc25-147">Så vi vill gärna tooread hello händelsedata och filtrera bort hello fält som vi vill.</span><span class="sxs-lookup"><span data-stu-id="7dc25-147">So we'd like tooread hello event data and filter out hello fields we want.</span></span> <span data-ttu-id="7dc25-148">Det finns många olika sätt som vi kan göra med hello data, men vår plan är idag toouse Stream Analytics toomove hello data tooa SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7dc25-148">There are all kinds of things we could do with hello data, but our plan today is toouse Stream Analytics toomove hello data tooa SQL database.</span></span> <span data-ttu-id="7dc25-149">Som gör det enkelt toorun mängd intressanta frågor.</span><span class="sxs-lookup"><span data-stu-id="7dc25-149">That will make it easy toorun lots of interesting queries.</span></span>

## <a name="create-an-azure-sql-database"></a><span data-ttu-id="7dc25-150">Skapa en Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="7dc25-150">Create an Azure SQL Database</span></span>
<span data-ttu-id="7dc25-151">Igen från din prenumeration i [Azure-portalen][portal], skapa hello-databas (och en ny server om du har redan en) toowhich skriver du hello data.</span><span class="sxs-lookup"><span data-stu-id="7dc25-151">Once again starting from your subscription in [Azure portal][portal], create hello database (and a new server, unless you've already got one) toowhich you'll write hello data.</span></span>

![Nya Data, SQL](./media/app-insights-code-sample-export-sql-stream-analytics/090-sql.png)

<span data-ttu-id="7dc25-153">Kontrollera att databasservern hello tillåter åtkomst tooAzure tjänster:</span><span class="sxs-lookup"><span data-stu-id="7dc25-153">Make sure that hello database server allows access tooAzure services:</span></span>

![Bläddra, servrar, din server, inställningar, brandvägg, Tillåt åtkomst tooAzure](./media/app-insights-code-sample-export-sql-stream-analytics/100-sqlaccess.png)

## <a name="create-a-table-in-azure-sql-db"></a><span data-ttu-id="7dc25-155">Skapa en tabell i Azure SQL DB</span><span class="sxs-lookup"><span data-stu-id="7dc25-155">Create a table in Azure SQL DB</span></span>
<span data-ttu-id="7dc25-156">Ansluta toohello databas som skapats i hello föregående avsnitt med din önskade hanteringsverktyg.</span><span class="sxs-lookup"><span data-stu-id="7dc25-156">Connect toohello database created in hello previous section with your preferred management tool.</span></span> <span data-ttu-id="7dc25-157">I den här genomgången ska vi använda [SQL Server Management Tools](https://msdn.microsoft.com/ms174173.aspx) (SSMS).</span><span class="sxs-lookup"><span data-stu-id="7dc25-157">In this walkthrough, we will be using [SQL Server Management Tools](https://msdn.microsoft.com/ms174173.aspx) (SSMS).</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/31-sql-table.png)

<span data-ttu-id="7dc25-158">Skapa en ny fråga och kör följande T-SQL hello:</span><span class="sxs-lookup"><span data-stu-id="7dc25-158">Create a new query, and execute hello following T-SQL:</span></span>

```SQL

CREATE TABLE [dbo].[PageViewsTable](
    [pageName] [nvarchar](max) NOT NULL,
    [viewCount] [int] NOT NULL,
    [url] [nvarchar](max) NULL,
    [urlDataPort] [int] NULL,
    [urlDataprotocol] [nvarchar](50) NULL,
    [urlDataHost] [nvarchar](50) NULL,
    [urlDataBase] [nvarchar](50) NULL,
    [urlDataHashTag] [nvarchar](max) NULL,
    [eventTime] [datetime] NOT NULL,
    [isSynthetic] [nvarchar](50) NULL,
    [deviceId] [nvarchar](50) NULL,
    [deviceType] [nvarchar](50) NULL,
    [os] [nvarchar](50) NULL,
    [osVersion] [nvarchar](50) NULL,
    [locale] [nvarchar](50) NULL,
    [userAgent] [nvarchar](max) NULL,
    [browser] [nvarchar](50) NULL,
    [browserVersion] [nvarchar](50) NULL,
    [screenResolution] [nvarchar](50) NULL,
    [sessionId] [nvarchar](max) NULL,
    [sessionIsFirst] [nvarchar](50) NULL,
    [clientIp] [nvarchar](50) NULL,
    [continent] [nvarchar](50) NULL,
    [country] [nvarchar](50) NULL,
    [province] [nvarchar](50) NULL,
    [city] [nvarchar](50) NULL
)

CREATE CLUSTERED INDEX [pvTblIdx] ON [dbo].[PageViewsTable]
(
    [eventTime] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON)

```

![](./media/app-insights-code-sample-export-sql-stream-analytics/34-create-table.png)

<span data-ttu-id="7dc25-159">I det här exemplet använder data från sidvisningar.</span><span class="sxs-lookup"><span data-stu-id="7dc25-159">In this sample, we are using data from page views.</span></span> <span data-ttu-id="7dc25-160">toosee hello andra data är tillgängliga och inspektera JSON-utdata finns hello [exportera datamodellen](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="7dc25-160">toosee hello other data available, inspect your JSON output, and see hello [export data model](app-insights-export-data-model.md).</span></span>

## <a name="create-an-azure-stream-analytics-instance"></a><span data-ttu-id="7dc25-161">Skapa en Azure Stream Analytics-instans</span><span class="sxs-lookup"><span data-stu-id="7dc25-161">Create an Azure Stream Analytics instance</span></span>
<span data-ttu-id="7dc25-162">Från hello [klassiska Azure-portalen](https://manage.windowsazure.com/), Välj hello Azure Stream Analytics-tjänsten och skapa ett nytt Stream Analytics-jobb:</span><span class="sxs-lookup"><span data-stu-id="7dc25-162">From hello [Classic Azure Portal](https://manage.windowsazure.com/), select hello Azure Stream Analytics service, and create a new Stream Analytics job:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/37-create-stream-analytics.png)

![](./media/app-insights-code-sample-export-sql-stream-analytics/38-create-stream-analytics-form.png)

<span data-ttu-id="7dc25-163">Expandera information när hello nytt jobb skapas:</span><span class="sxs-lookup"><span data-stu-id="7dc25-163">When hello new job is created, expand its details:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/41-sa-job.png)

#### <a name="set-blob-location"></a><span data-ttu-id="7dc25-164">Ange blob-plats</span><span class="sxs-lookup"><span data-stu-id="7dc25-164">Set blob location</span></span>
<span data-ttu-id="7dc25-165">Ange den tootake indata från din löpande Export blob:</span><span class="sxs-lookup"><span data-stu-id="7dc25-165">Set it tootake input from your Continuous Export blob:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/42-sa-wizard1.png)

<span data-ttu-id="7dc25-166">Nu behöver hello primärnyckel åtkomst från ditt Lagringskonto som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="7dc25-166">Now you'll need hello Primary Access Key from your Storage Account, which you noted earlier.</span></span> <span data-ttu-id="7dc25-167">Ange som hello Lagringskontonyckel.</span><span class="sxs-lookup"><span data-stu-id="7dc25-167">Set this as hello Storage Account Key.</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/46-sa-wizard2.png)

#### <a name="set-path-prefix-pattern"></a><span data-ttu-id="7dc25-168">Ange sökvägar för prefix</span><span class="sxs-lookup"><span data-stu-id="7dc25-168">Set path prefix pattern</span></span>
![](./media/app-insights-code-sample-export-sql-stream-analytics/47-sa-wizard3.png)

<span data-ttu-id="7dc25-169">Vara säker på att tooset hello datumformatet för**åååå-MM-DD** (med **streck**).</span><span class="sxs-lookup"><span data-stu-id="7dc25-169">Be sure tooset hello Date Format too**YYYY-MM-DD** (with **dashes**).</span></span>

<span data-ttu-id="7dc25-170">hello prefixet sökvägar anger hur Stream Analytics hittar hello indatafiler i hello lagring.</span><span class="sxs-lookup"><span data-stu-id="7dc25-170">hello Path Prefix Pattern specifies how Stream Analytics finds hello input files in hello storage.</span></span> <span data-ttu-id="7dc25-171">Du behöver tooset den toocorrespond toohow löpande Export lagrar hello data.</span><span class="sxs-lookup"><span data-stu-id="7dc25-171">You need tooset it toocorrespond toohow Continuous Export stores hello data.</span></span> <span data-ttu-id="7dc25-172">Ställ in den så här:</span><span class="sxs-lookup"><span data-stu-id="7dc25-172">Set it like this:</span></span>

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

<span data-ttu-id="7dc25-173">I det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="7dc25-173">In this example:</span></span>

* <span data-ttu-id="7dc25-174">`webapplication27`är hello namnet på hello Application Insights-resurs **i gemen**.</span><span class="sxs-lookup"><span data-stu-id="7dc25-174">`webapplication27` is hello name of hello Application Insights resource, **all in lower case**.</span></span> 
* <span data-ttu-id="7dc25-175">`1234...`är hello instrumentation nyckel för hello Application Insights-resurs **med bindestreck bort**.</span><span class="sxs-lookup"><span data-stu-id="7dc25-175">`1234...` is hello instrumentation key of hello Application Insights resource **with dashes removed**.</span></span> 
* <span data-ttu-id="7dc25-176">`PageViews`hello typ av data som vi vill tooanalyze.</span><span class="sxs-lookup"><span data-stu-id="7dc25-176">`PageViews` is hello type of data we want tooanalyze.</span></span> <span data-ttu-id="7dc25-177">hello tillgängliga typer är beroende av hello filter som du angett i löpande Export.</span><span class="sxs-lookup"><span data-stu-id="7dc25-177">hello available types depend on hello filter you set in Continuous Export.</span></span> <span data-ttu-id="7dc25-178">Granska hello exporterade data toosee hello andra tillgängliga typer och se hello [exportera datamodellen](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="7dc25-178">Examine hello exported data toosee hello other available types, and see hello [export data model](app-insights-export-data-model.md).</span></span>
* <span data-ttu-id="7dc25-179">`/{date}/{time}`ett mönster skrivs bokstavligt.</span><span class="sxs-lookup"><span data-stu-id="7dc25-179">`/{date}/{time}` is a pattern written literally.</span></span>

<span data-ttu-id="7dc25-180">tooget hello namn och iKey av Application Insights-resurs öppna Essentials på dess översiktssidan eller öppna inställningar.</span><span class="sxs-lookup"><span data-stu-id="7dc25-180">tooget hello name and iKey of your Application Insights resource, open Essentials on its overview page, or open Settings.</span></span>

#### <a name="finish-initial-setup"></a><span data-ttu-id="7dc25-181">Slut installationen</span><span class="sxs-lookup"><span data-stu-id="7dc25-181">Finish initial setup</span></span>
<span data-ttu-id="7dc25-182">Bekräfta hello serialiseringsformat:</span><span class="sxs-lookup"><span data-stu-id="7dc25-182">Confirm hello serialization format:</span></span>

![Bekräfta och Stäng guiden](./media/app-insights-code-sample-export-sql-stream-analytics/48-sa-wizard4.png)

<span data-ttu-id="7dc25-184">Stäng guiden hello och vänta hello installationsprogrammet toocomplete.</span><span class="sxs-lookup"><span data-stu-id="7dc25-184">Close hello wizard and wait for hello setup toocomplete.</span></span>

> [!TIP]
> <span data-ttu-id="7dc25-185">Använd hello exempel funktionen toocheck att du har angett korrekt hello inkommande sökväg.</span><span class="sxs-lookup"><span data-stu-id="7dc25-185">Use hello Sample function toocheck that you have set hello input path correctly.</span></span> <span data-ttu-id="7dc25-186">Om det misslyckas: Kontrollera att det finns data i hello lagring för hello exempel tidsintervall som du har valt.</span><span class="sxs-lookup"><span data-stu-id="7dc25-186">If it fails: Check that there is data in hello storage for hello sample time range you chose.</span></span> <span data-ttu-id="7dc25-187">Redigera hello inkommande definition och kontrollera du ange hello storage-konto, sökväg prefix och datumformat korrekt.</span><span class="sxs-lookup"><span data-stu-id="7dc25-187">Edit hello input definition and check you set hello storage account, path prefix and date format correctly.</span></span>
> 
> 

## <a name="set-query"></a><span data-ttu-id="7dc25-188">Ange fråga</span><span class="sxs-lookup"><span data-stu-id="7dc25-188">Set query</span></span>
<span data-ttu-id="7dc25-189">Öppna hello frågeavsnitt:</span><span class="sxs-lookup"><span data-stu-id="7dc25-189">Open hello query section:</span></span>

![Välj frågan i stream analytics](./media/app-insights-code-sample-export-sql-stream-analytics/51-query.png)

<span data-ttu-id="7dc25-191">Ersätt hello standardfråga med:</span><span class="sxs-lookup"><span data-stu-id="7dc25-191">Replace hello default query with:</span></span>

```SQL

    SELECT flat.ArrayValue.name as pageName
    , flat.ArrayValue.count as viewCount
    , flat.ArrayValue.url as url
    , flat.ArrayValue.urlData.port as urlDataPort
    , flat.ArrayValue.urlData.protocol as urlDataprotocol
    , flat.ArrayValue.urlData.host as urlDataHost
    , flat.ArrayValue.urlData.base as urlDataBase
    , flat.ArrayValue.urlData.hashTag as urlDataHashTag
      ,A.context.data.eventTime as eventTime
      ,A.context.data.isSynthetic as isSynthetic
      ,A.context.device.id as deviceId
      ,A.context.device.type as deviceType
      ,A.context.device.os as os
      ,A.context.device.osVersion as osVersion
      ,A.context.device.locale as locale
      ,A.context.device.userAgent as userAgent
      ,A.context.device.browser as browser
      ,A.context.device.browserVersion as browserVersion
      ,A.context.device.screenResolution.value as screenResolution
      ,A.context.session.id as sessionId
      ,A.context.session.isFirst as sessionIsFirst
      ,A.context.location.clientip as clientIp
      ,A.context.location.continent as continent
      ,A.context.location.country as country
      ,A.context.location.province as province
      ,A.context.location.city as city
    INTO
      AIOutput
    FROM AIinput A
    CROSS APPLY GetElements(A.[view]) as flat


```

<span data-ttu-id="7dc25-192">Observera att hello först få egenskaper är särskilda toopage visa data.</span><span class="sxs-lookup"><span data-stu-id="7dc25-192">Notice that hello first few properties are specific toopage view data.</span></span> <span data-ttu-id="7dc25-193">Export av andra telemetri typer har olika egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7dc25-193">Exports of other telemetry types will have different properties.</span></span> <span data-ttu-id="7dc25-194">Se hello [detaljerad data modellreferens för hello egenskapstyper och värden.](app-insights-export-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="7dc25-194">See hello [detailed data model reference for hello property types and values.](app-insights-export-data-model.md)</span></span>

## <a name="set-up-output-toodatabase"></a><span data-ttu-id="7dc25-195">Ställ in utdata toodatabase</span><span class="sxs-lookup"><span data-stu-id="7dc25-195">Set up output toodatabase</span></span>
<span data-ttu-id="7dc25-196">Välj SQL som hello utdata.</span><span class="sxs-lookup"><span data-stu-id="7dc25-196">Select SQL as hello output.</span></span>

![Välj utdata i stream analytics](./media/app-insights-code-sample-export-sql-stream-analytics/53-store.png)

<span data-ttu-id="7dc25-198">Ange hello SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7dc25-198">Specify hello SQL database.</span></span>

![Fyll i hello information för din databas](./media/app-insights-code-sample-export-sql-stream-analytics/55-output.png)

<span data-ttu-id="7dc25-200">Stäng guiden hello och vänta tills ett meddelande om att hello utdata har ställts in.</span><span class="sxs-lookup"><span data-stu-id="7dc25-200">Close hello wizard and wait for a notification that hello output has been set up.</span></span>

## <a name="start-processing"></a><span data-ttu-id="7dc25-201">Starta bearbetning</span><span class="sxs-lookup"><span data-stu-id="7dc25-201">Start processing</span></span>
<span data-ttu-id="7dc25-202">Starta hello jobbet från hello Åtgärdsfältet:</span><span class="sxs-lookup"><span data-stu-id="7dc25-202">Start hello job from hello action bar:</span></span>

![Klicka på Start i stream analytics](./media/app-insights-code-sample-export-sql-stream-analytics/61-start.png)

<span data-ttu-id="7dc25-204">Du kan välja om toostart bearbetning hello data från och med nu, eller toostart med tidigare data.</span><span class="sxs-lookup"><span data-stu-id="7dc25-204">You can choose whether toostart processing hello data starting from now, or toostart with earlier data.</span></span> <span data-ttu-id="7dc25-205">hello senare är användbart om du har haft löpande Export redan körs på ett tag.</span><span class="sxs-lookup"><span data-stu-id="7dc25-205">hello latter is useful if you have had Continuous Export already running for a while.</span></span>

![Klicka på Start i stream analytics](./media/app-insights-code-sample-export-sql-stream-analytics/63-start.png)

<span data-ttu-id="7dc25-207">Gå tillbaka tooSQL serverhanteringsverktyg och titta på hello data flödar i efter några minuter.</span><span class="sxs-lookup"><span data-stu-id="7dc25-207">After a few minutes, go back tooSQL Server Management Tools and watch hello data flowing in.</span></span> <span data-ttu-id="7dc25-208">Till exempel använda en fråga så här:</span><span class="sxs-lookup"><span data-stu-id="7dc25-208">For example, use a query like this:</span></span>

    SELECT TOP 100 *
    FROM [dbo].[PageViewsTable]


## <a name="related-articles"></a><span data-ttu-id="7dc25-209">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="7dc25-209">Related articles</span></span>
* [<span data-ttu-id="7dc25-210">Exportera tooPowerBI med Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="7dc25-210">Export tooPowerBI using Stream Analytics</span></span>](app-insights-export-power-bi.md)
* [<span data-ttu-id="7dc25-211">Detaljerad datamodell referens för hello egenskapstyper och värden.</span><span class="sxs-lookup"><span data-stu-id="7dc25-211">Detailed data model reference for hello property types and values.</span></span>](app-insights-export-data-model.md)
* [<span data-ttu-id="7dc25-212">Löpande Export i Application Insights</span><span class="sxs-lookup"><span data-stu-id="7dc25-212">Continuous Export in Application Insights</span></span>](app-insights-export-telemetry.md)
* [<span data-ttu-id="7dc25-213">Application Insights</span><span class="sxs-lookup"><span data-stu-id="7dc25-213">Application Insights</span></span>](https://azure.microsoft.com/services/application-insights/)

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[export]: app-insights-export-telemetry.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[start]: app-insights-overview.md

