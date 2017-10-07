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
# <a name="walkthrough-export-toosql-from-application-insights-using-stream-analytics"></a>Genomgång: Exportera tooSQL från Application Insights med Stream Analytics
Den här artikeln visar hur toomove telemetridata från [Azure Application Insights] [ start] till en Azure SQL-databas med hjälp av [löpande Export] [ export] och [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). 

Löpande export flyttar telemetridata till Azure Storage i JSON-format. Vi parsa hello JSON-objekt med Azure Stream Analytics och skapa rader i en databastabell.

(Mer generellt löpande Export har hello sätt toodo analyser om hello telemetri dina appar skicka tooApplication insikter. Du kan anpassa den här koden exempel toodo andra saker med hello exporteras telemetri, till exempel aggregering av data.)

Vi börjar med hello antagandet att du redan har hello-app som du vill toomonitor.

I det här exemplet ska vi använda hello data om sidvisningar, men hello samma mönster kan enkelt utökas tooother datatyper, till exempel anpassade händelser och undantag. 

## <a name="add-application-insights-tooyour-application"></a>Lägg till Application Insights tooyour program
tooget igång:

1. [Konfigurera Application Insights för webbsidor](app-insights-javascript.md). 
   
    (I det här exemplet ska vi fokusera på bearbetning av data om sidvisningar från hello klientwebbläsare, men du kan också ställa in Programinsikter för hello serversidan av din [Java](app-insights-java-get-started.md) eller [ASP.NET](app-insights-asp-net.md) appen och processen för begäran beroende- och andra server telemetri.)
2. Publicera din app och titta på telemetridata som visas i Application Insights-resurs.

## <a name="create-storage-in-azure"></a>Skapa lagring i Azure
Löpande export alltid matar ut data tooan Azure Storage-konto, så du behöver toocreate hello först.

1. Skapa ett lagringskonto i din prenumeration i hello [Azure-portalen][portal].
   
    ![Välj ny, Data, lagring i Azure-portalen. Välj klassisk, väljer du skapa. Ange ett namn för lagring.](./media/app-insights-code-sample-export-sql-stream-analytics/040-store.png)
2. Skapa en behållare
   
    ![I hello nya lagringsenheter, Välj behållare, klicka på hello behållare panelen och Lägg till](./media/app-insights-code-sample-export-sql-stream-analytics/050-container.png)
3. Kopiera hello lagringsåtkomstnyckel
   
    Du behöver den snart tooset in hello inkommande toohello stream analytics-tjänsten.
   
    ![Öppna inställningar, nycklar, i hello lagring och göra en kopia av hello primära åtkomstnyckeln](./media/app-insights-code-sample-export-sql-stream-analytics/21-storage-key.png)

## <a name="start-continuous-export-tooazure-storage"></a>Starta löpande export tooAzure lagring
1. Hello Azure-portalen, bläddra toohello Application Insights-resurs som du har skapat för ditt program.
   
    ![Klicka på Bläddra, Application Insights för ditt program](./media/app-insights-code-sample-export-sql-stream-analytics/060-browse.png)
2. Skapa en löpande export.
   
    ![Välj Lägg till inställningar för löpande Export](./media/app-insights-code-sample-export-sql-stream-analytics/070-export.png)

    Välj hello storage-konto som du skapade tidigare:

    ![Ange hello export mål](./media/app-insights-code-sample-export-sql-stream-analytics/080-add.png)

    Ange hello händelsetyper som du vill toosee:

    ![Välj händelsetyper](./media/app-insights-code-sample-export-sql-stream-analytics/085-types.png)


1. Kan vissa data ackumuleras. Sitta tillbaka och låta personer använder programmet ett tag. Telemetri kommer så ser du statistiska diagram i [mått explorer](app-insights-metrics-explorer.md) och enskilda händelser i [diagnostiska Sök](app-insights-diagnostic-search.md). 
   
    Och även hello data kommer att exportera tooyour lagring. 
2. Inspektera hello exporterade data, antingen i hello portal - Välj **Bläddra**, Välj ditt lagringskonto och sedan **behållare** - eller i Visual Studio. I Visual Studio väljer **visa / Cloud Explorer**, och öppna Azure / lagring. (Om du inte har alternativet, måste tooinstall hello Azure SDK: öppna dialogrutan för hello nytt projekt och Visual C# / molnet / hämta Microsoft Azure SDK för .NET.)
   
    ![Öppna i Visual Studio, servrar, Azure, lagring](./media/app-insights-code-sample-export-sql-stream-analytics/087-explorer.png)
   
    Anteckna hello vanliga tillhör hello sökvägsnamn som härleds från hello namn och instrumentation tangent. 

hello-händelser skrivs tooblob filer i JSON-format. Varje fil kan innehålla en eller flera händelser. Så vi vill gärna tooread hello händelsedata och filtrera bort hello fält som vi vill. Det finns många olika sätt som vi kan göra med hello data, men vår plan är idag toouse Stream Analytics toomove hello data tooa SQL-databas. Som gör det enkelt toorun mängd intressanta frågor.

## <a name="create-an-azure-sql-database"></a>Skapa en Azure SQL Database
Igen från din prenumeration i [Azure-portalen][portal], skapa hello-databas (och en ny server om du har redan en) toowhich skriver du hello data.

![Nya Data, SQL](./media/app-insights-code-sample-export-sql-stream-analytics/090-sql.png)

Kontrollera att databasservern hello tillåter åtkomst tooAzure tjänster:

![Bläddra, servrar, din server, inställningar, brandvägg, Tillåt åtkomst tooAzure](./media/app-insights-code-sample-export-sql-stream-analytics/100-sqlaccess.png)

## <a name="create-a-table-in-azure-sql-db"></a>Skapa en tabell i Azure SQL DB
Ansluta toohello databas som skapats i hello föregående avsnitt med din önskade hanteringsverktyg. I den här genomgången ska vi använda [SQL Server Management Tools](https://msdn.microsoft.com/ms174173.aspx) (SSMS).

![](./media/app-insights-code-sample-export-sql-stream-analytics/31-sql-table.png)

Skapa en ny fråga och kör följande T-SQL hello:

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

I det här exemplet använder data från sidvisningar. toosee hello andra data är tillgängliga och inspektera JSON-utdata finns hello [exportera datamodellen](app-insights-export-data-model.md).

## <a name="create-an-azure-stream-analytics-instance"></a>Skapa en Azure Stream Analytics-instans
Från hello [klassiska Azure-portalen](https://manage.windowsazure.com/), Välj hello Azure Stream Analytics-tjänsten och skapa ett nytt Stream Analytics-jobb:

![](./media/app-insights-code-sample-export-sql-stream-analytics/37-create-stream-analytics.png)

![](./media/app-insights-code-sample-export-sql-stream-analytics/38-create-stream-analytics-form.png)

Expandera information när hello nytt jobb skapas:

![](./media/app-insights-code-sample-export-sql-stream-analytics/41-sa-job.png)

#### <a name="set-blob-location"></a>Ange blob-plats
Ange den tootake indata från din löpande Export blob:

![](./media/app-insights-code-sample-export-sql-stream-analytics/42-sa-wizard1.png)

Nu behöver hello primärnyckel åtkomst från ditt Lagringskonto som du antecknade tidigare. Ange som hello Lagringskontonyckel.

![](./media/app-insights-code-sample-export-sql-stream-analytics/46-sa-wizard2.png)

#### <a name="set-path-prefix-pattern"></a>Ange sökvägar för prefix
![](./media/app-insights-code-sample-export-sql-stream-analytics/47-sa-wizard3.png)

Vara säker på att tooset hello datumformatet för**åååå-MM-DD** (med **streck**).

hello prefixet sökvägar anger hur Stream Analytics hittar hello indatafiler i hello lagring. Du behöver tooset den toocorrespond toohow löpande Export lagrar hello data. Ställ in den så här:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

I det här exemplet:

* `webapplication27`är hello namnet på hello Application Insights-resurs **i gemen**. 
* `1234...`är hello instrumentation nyckel för hello Application Insights-resurs **med bindestreck bort**. 
* `PageViews`hello typ av data som vi vill tooanalyze. hello tillgängliga typer är beroende av hello filter som du angett i löpande Export. Granska hello exporterade data toosee hello andra tillgängliga typer och se hello [exportera datamodellen](app-insights-export-data-model.md).
* `/{date}/{time}`ett mönster skrivs bokstavligt.

tooget hello namn och iKey av Application Insights-resurs öppna Essentials på dess översiktssidan eller öppna inställningar.

#### <a name="finish-initial-setup"></a>Slut installationen
Bekräfta hello serialiseringsformat:

![Bekräfta och Stäng guiden](./media/app-insights-code-sample-export-sql-stream-analytics/48-sa-wizard4.png)

Stäng guiden hello och vänta hello installationsprogrammet toocomplete.

> [!TIP]
> Använd hello exempel funktionen toocheck att du har angett korrekt hello inkommande sökväg. Om det misslyckas: Kontrollera att det finns data i hello lagring för hello exempel tidsintervall som du har valt. Redigera hello inkommande definition och kontrollera du ange hello storage-konto, sökväg prefix och datumformat korrekt.
> 
> 

## <a name="set-query"></a>Ange fråga
Öppna hello frågeavsnitt:

![Välj frågan i stream analytics](./media/app-insights-code-sample-export-sql-stream-analytics/51-query.png)

Ersätt hello standardfråga med:

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

Observera att hello först få egenskaper är särskilda toopage visa data. Export av andra telemetri typer har olika egenskaper. Se hello [detaljerad data modellreferens för hello egenskapstyper och värden.](app-insights-export-data-model.md)

## <a name="set-up-output-toodatabase"></a>Ställ in utdata toodatabase
Välj SQL som hello utdata.

![Välj utdata i stream analytics](./media/app-insights-code-sample-export-sql-stream-analytics/53-store.png)

Ange hello SQL-databas.

![Fyll i hello information för din databas](./media/app-insights-code-sample-export-sql-stream-analytics/55-output.png)

Stäng guiden hello och vänta tills ett meddelande om att hello utdata har ställts in.

## <a name="start-processing"></a>Starta bearbetning
Starta hello jobbet från hello Åtgärdsfältet:

![Klicka på Start i stream analytics](./media/app-insights-code-sample-export-sql-stream-analytics/61-start.png)

Du kan välja om toostart bearbetning hello data från och med nu, eller toostart med tidigare data. hello senare är användbart om du har haft löpande Export redan körs på ett tag.

![Klicka på Start i stream analytics](./media/app-insights-code-sample-export-sql-stream-analytics/63-start.png)

Gå tillbaka tooSQL serverhanteringsverktyg och titta på hello data flödar i efter några minuter. Till exempel använda en fråga så här:

    SELECT TOP 100 *
    FROM [dbo].[PageViewsTable]


## <a name="related-articles"></a>Relaterade artiklar
* [Exportera tooPowerBI med Stream Analytics](app-insights-export-power-bi.md)
* [Detaljerad datamodell referens för hello egenskapstyper och värden.](app-insights-export-data-model.md)
* [Löpande Export i Application Insights](app-insights-export-telemetry.md)
* [Application Insights](https://azure.microsoft.com/services/application-insights/)

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[export]: app-insights-export-telemetry.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[start]: app-insights-overview.md

