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
# <a name="use-stream-analytics-tooprocess-exported-data-from-application-insights"></a>Använd Stream Analytics tooprocess exporterade data från Application Insights
[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) är hello perfekt verktyg för databearbetning [exporterats från Application Insights](app-insights-export-telemetry.md). Stream Analytics kan hämta data från olika källor. Det kan omvandla filtrera hello data och sedan dirigera tooa mängd sänkor.

I det här exemplet ska vi skapa en adapter som hämtar data från Application Insights, byter namn på och behandlar hello fälten och kommer till Power BI.

> [!WARNING]
> Det finns mycket bättre och enklare [rekommenderade sätt toodisplay Application Insights-data i Power BI](app-insights-export-power-bi.md). hello sökvägen som visas här är bara ett exempel tooillustrate hur tooprocess exporterade data.
> 
> 

![Blockdiagram för export via SA tooPBI](./media/app-insights-export-stream-analytics/020.png)

## <a name="create-storage-in-azure"></a>Skapa lagring i Azure
Löpande export alltid matar ut data tooan Azure Storage-konto, så du behöver toocreate hello först.

1. Skapa ett ”klassisk” storage-konto i din prenumeration i hello [Azure-portalen](https://portal.azure.com).
   
   ![Välj ny, Data, lagring i Azure-portalen](./media/app-insights-export-stream-analytics/030.png)
2. Skapa en behållare
   
    ![I hello nya lagringsenheter, Välj behållare, klicka på hello behållare panelen och Lägg till](./media/app-insights-export-stream-analytics/040.png)
3. Kopiera hello lagringsåtkomstnyckel
   
    Du behöver den snart tooset in hello inkommande toohello stream analytics-tjänsten.
   
    ![Öppna inställningar, nycklar, i hello lagring och göra en kopia av hello primära åtkomstnyckeln](./media/app-insights-export-stream-analytics/045.png)

## <a name="start-continuous-export-tooazure-storage"></a>Starta löpande export tooAzure lagring
[Löpande export](app-insights-export-telemetry.md) flyttar data från Application Insights i Azure storage.

1. Hello Azure-portalen, bläddra toohello Application Insights-resurs som du har skapat för ditt program.
   
    ![Klicka på Bläddra, Application Insights för ditt program](./media/app-insights-export-stream-analytics/050.png)
2. Skapa en löpande export.
   
    ![Välj Lägg till inställningar för löpande Export](./media/app-insights-export-stream-analytics/060.png)

    Välj hello storage-konto som du skapade tidigare:

    ![Ange hello export mål](./media/app-insights-export-stream-analytics/070.png)

    Ange hello händelsetyper som du vill toosee:

    ![Välj händelsetyper](./media/app-insights-export-stream-analytics/080.png)

1. Kan vissa data ackumuleras. Sitta tillbaka och låta personer använder programmet ett tag. Telemetri kommer så ser du statistiska diagram i [mått explorer](app-insights-metrics-explorer.md) och enskilda händelser i [diagnostiska Sök](app-insights-diagnostic-search.md). 
   
    Och även hello data kommer att exportera tooyour lagring. 
2. Kontrollera hello exporterade data. I Visual Studio väljer **visa / Cloud Explorer**, och öppna Azure / lagring. (Om du inte har alternativet, måste tooinstall hello Azure SDK: öppna dialogrutan för hello nytt projekt och Visual C# / molnet / hämta Microsoft Azure SDK för .NET.)
   
    ![](./media/app-insights-export-stream-analytics/04-data.png)
   
    Anteckna hello vanliga tillhör hello sökvägsnamn som härleds från hello namn och instrumentation tangent. 

hello-händelser skrivs tooblob filer i JSON-format. Varje fil kan innehålla en eller flera händelser. Så vi vill gärna tooread hello händelsedata och filtrera bort hello fält som vi vill. Det finns många olika sätt som vi kan göra med hello data, men vår plan är idag toouse Stream Analytics toopipe hello data tooPower BI.

## <a name="create-an-azure-stream-analytics-instance"></a>Skapa en Azure Stream Analytics-instans
Från hello [klassiska Azure-portalen](https://manage.windowsazure.com/), Välj hello Azure Stream Analytics-tjänsten och skapa ett nytt Stream Analytics-jobb:

![](./media/app-insights-export-stream-analytics/090.png)

![](./media/app-insights-export-stream-analytics/100.png)

Expandera information när hello nytt jobb skapas:

![](./media/app-insights-export-stream-analytics/110.png)

### <a name="set-blob-location"></a>Ange blob-plats
Ange den tootake indata från din löpande Export blob:

![](./media/app-insights-export-stream-analytics/120.png)

Nu behöver hello primärnyckel åtkomst från ditt Lagringskonto som du antecknade tidigare. Ange som hello Lagringskontonyckel.

![](./media/app-insights-export-stream-analytics/130.png)

### <a name="set-path-prefix-pattern"></a>Ange sökvägar för prefix
![](./media/app-insights-export-stream-analytics/140.png)

**Vara säker på att tooset hello datumformat tooYYYY-MM-DD (med bindestreck).**

hello prefixet sökvägar anger om Stream Analytics finner hello indatafiler i hello lagring. Du behöver tooset den toocorrespond toohow löpande Export lagrar hello data. Ställ in den så här:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

I det här exemplet:

* `webapplication27`är hello namnet på hello Application Insights-resurs **alla gemen**.
* `1234...`är hello instrumentation nyckel för hello Application Insights-resurs **utesluter streck**. 
* `PageViews`hello typ av data du vill tooanalyze. hello tillgängliga typer är beroende av hello filter som du angett i löpande Export. Granska hello exporterade data toosee hello andra tillgängliga typer och se hello [exportera datamodellen](app-insights-export-data-model.md).
* `/{date}/{time}`ett mönster skrivs bokstavligt.

> [!NOTE]
> Kontrollera hello lagring toomake till att du får hello sökvägen rätt.
> 
> 

### <a name="finish-initial-setup"></a>Slut installationen
Bekräfta hello serialiseringsformat:

![Bekräfta och Stäng guiden](./media/app-insights-export-stream-analytics/150.png)

Stäng guiden hello och vänta hello installationsprogrammet toocomplete.

> [!TIP]
> Använd hello exempel kommandot toodownload vissa data. Behåll det som ett test exempel toodebug din fråga.
> 
> 

## <a name="set-hello-output"></a>Ange hello utdata
Välj ditt jobb och ange hello utdata.

![Välj hello ny kanal, klicka på utdata, Lägg till, Power BI](./media/app-insights-export-stream-analytics/160.png)

Ange din **arbets- eller skolkonto** tooauthorize Stream Analytics tooaccess Power BI-resurs. Skriv sedan ett namn för hello utdata och för hello mål Power BI dataset och tabellen.

![Tjänsten tre namn](./media/app-insights-export-stream-analytics/170.png)

## <a name="set-hello-query"></a>Ange hello fråga
hello frågan styr hello översättning av inkommande toooutput.

![Välj hello jobbet och klicka på frågan. Klistra in hello exemplet nedan.](./media/app-insights-export-stream-analytics/180.png)

Använd hello testa funktionen toocheck att du får rätt hello-utdata. Ge den hello exempeldata som du tog från hello indata sida. 

### <a name="query-toodisplay-counts-of-events"></a>Frågan toodisplay antal händelser
Klistra in den här frågan:

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

* export-indata är hello-alias som vi berättade toohello dataström
* pbi-utdata är hello kolumnalias vi har definierat
* Vi använder [yttre gäller GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) eftersom hello händelsenamnet i en kapslad JSON-arrray. Sedan hello väljer plockningar hello händelsenamnet tillsammans med antalet hello antal instanser med det namnet i hello tidsperiod. Hej [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) satsen grupper hello element på perioder på 1 minut.

### <a name="query-toodisplay-metric-values"></a>Frågan toodisplay måttvärden
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

* Den här frågan går hello mått telemetri tooget hello händelsetid och hello värde. hello måttvärden är i en matris, så vi använder hello yttre gäller GetElements mönster tooextract hello rader. ”myMetric” är hello mått hello namn i det här fallet. 

### <a name="query-tooinclude-values-of-dimension-properties"></a>Frågan tooinclude värdena för dimensionsegenskaper
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

* Den här frågan innehåller värden för hello dimensionsegenskaper utan beroende på en viss dimension som befinner sig på en fast index i hello dimensionsmatris.

## <a name="run-hello-job"></a>Kör jobb för hello
Du kan välja ett datum i hello tidigare toostart hello jobbet från. 

![Välj hello jobbet och klicka på frågan. Klistra in hello exemplet nedan.](./media/app-insights-export-stream-analytics/190.png)

Vänta tills hello jobbet körs.

## <a name="see-results-in-power-bi"></a>Visa resultatet i Power BI
> [!WARNING]
> Det finns mycket bättre och enklare [rekommenderade sätt toodisplay Application Insights-data i Power BI](app-insights-export-power-bi.md). hello sökvägen som visas här är bara ett exempel tooillustrate hur tooprocess exporterade data.
> 
> 

Öppna Power BI med ditt arbete eller skolans konto och väljer hello dataset och tabellen som du definierade som hello utdata från hello Stream Analytics-jobbet.

![Välj din datauppsättning och fält i Power BI.](./media/app-insights-export-stream-analytics/200.png)

Nu kan du använda den här datauppsättningen i rapporter och instrumentpaneler i [Power BI](https://powerbi.microsoft.com).

![Välj din datauppsättning och fält i Power BI.](./media/app-insights-export-stream-analytics/210.png)

## <a name="no-data"></a>Ser du inga data?
* Kontrollera att du [set hello datumformat](#set-path-prefix-pattern) korrekt tooYYYY-MM-DD (med bindestreck).

## <a name="video"></a>Video
Noam Ben Zeev visar hur tooprocess exporteras data med hjälp av Stream Analytics.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Export-to-Power-BI-from-Application-Insights/player]
> 
> 

## <a name="next-steps"></a>Nästa steg
* [Löpande export](app-insights-export-telemetry.md)
* [Detaljerad datamodell referens för hello egenskapstyper och värden.](app-insights-export-data-model.md)
* [Application Insights](app-insights-overview.md)

