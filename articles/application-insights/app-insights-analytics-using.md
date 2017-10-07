---
title: "aaaUsing Analytics - hello kraftfullt sökverktyg av Azure Application Insights | Microsoft Docs"
description: "Med hjälp av hello Analytics, hello kraftfullt diagnostiska sökverktyg av Application Insights. "
services: application-insights
documentationcenter: 
author: danhadari
manager: carmonm
ms.assetid: c3b34430-f592-4c32-b900-e9f50ca096b3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 6e0246848457db368c57d08c47b5bf73f4e5e3a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-analytics-in-application-insights"></a>Med hjälp av Analytics i Application Insights
[Analytics](app-insights-analytics.md) är hello kraftfulla sökfunktionen i [Programinsikter](app-insights-overview.md). Dessa sidor beskrivs Log Analytics-frågespråket.

* **[Se hello inledande video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Testkör Analytics på våra simulerade data](https://analytics.applicationinsights.io/demo)**  om din app inte skickar data tooApplication insikter ännu.

## <a name="open-analytics"></a>Öppna Analytics
Klicka på Analytics från din app hem resurs i Application Insights.

![Öppna portal.azure.com, öppna Application Insights-resursen och klicka på Analytics.](./media/app-insights-analytics-using/001.png)

hello infogade kursen får du några förslag på vad du kan göra.

Det finns en [mer omfattande rundtur här](app-insights-analytics-tour.md).

## <a name="query-your-telemetry"></a>Fråga din telemetri
### <a name="write-a-query"></a>Skriva en fråga
![Visa schema](./media/app-insights-analytics-using/150.png)

Börja med hello namnen på alla hello registren hello vänster (eller hello [intervallet](https://docs.loganalytics.io/queryLanguage/query_language_rangeoperator.html) eller [union](https://docs.loganalytics.io/queryLanguage/query_language_unionoperator.html) operatörer). Använd `|` toocreate en pipeline för [operatörer](https://docs.loganalytics.io/learn/cheatsheets/useful_operators.html). 

IntelliSense får du hello operatorer och hello Uttryckselement som du kan använda. Klicka på informationsikonen hello (eller trycker på CTRL + blanksteg) tooget en utförligare beskrivning och exempel på hur toouse varje element.

Se hello [Analytics språk rundtur](app-insights-analytics-tour.md) och [Språkreferens](app-insights-analytics-reference.md).

### <a name="run-a-query"></a>Köra en fråga
![Kör en fråga](./media/app-insights-analytics-using/130.png)

1. Du kan använda enkel radbrytningar i en fråga.
2. Placera hello markören inuti eller hello slutet av hello-fråga som du vill använda toorun.
3. Kontrollera hello tidsintervallet på din fråga. (Du kan ändra det eller åsidosätta den genom att lägga till egna [ `where...timestamp...` ](https://docs.loganalytics.io/concepts/concepts_datatypes_timespan.html) -sats i frågan.)
3. Klicka på Gå toorun hello fråga.
4. Placera inte tomma rader i frågan. Du kan behålla flera separata frågor i en fråga flik genom att avgränsa dem med tomma rader. Endast hello-fråga som har hello markören körs.

### <a name="save-a-query"></a>Spara en fråga
![Spara en fråga](./media/app-insights-analytics-using/140.png)

1. Spara hello aktuella frågefilen.
2. Öppna en sparad fråga-fil.
3. Skapa en ny fråga-fil.

## <a name="see-hello-details"></a>Se hello information
Expandera alla rader i hello resultat toosee den fullständiga listan över egenskaper. Ytterligare kan du expandera en egenskap som är strukturerade värde: till exempel, anpassade dimensioner eller hello stack lista i ett undantag.

![Expandera en rad](./media/app-insights-analytics-using/070.png)

## <a name="arrange-hello-results"></a>Ordna hello resultat
Du kan sortera, filtrera, sidbryta och gruppera hello resultaten som returnerades från frågan.

> [!NOTE]
> Sortering, gruppering och filtrering i hello webbläsare kör inte frågan igen. De endast ordna hello resultaten som returnerades av din senaste fråga. 
> 
> tooperform dessa uppgifter i hello server innan hello resultaten returneras, skriva en fråga med hello [sortera](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html), [sammanfatta](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) och [där](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) operatörer.
> 
> 

Välj hello kolumner som du skulle t.ex. toosee, dra kolumnen huvuden toorearrange dem och ändra storlek på kolumner genom att dra gränserna.

![Ordna kolumner](./media/app-insights-analytics-using/030.png)

### <a name="sort-and-filter-items"></a>Sortera och filtrera objekt
Sortera resultaten genom att klicka på hello head för en kolumn. Klicka igen toosort hello annat sätt och klicka på en tredje gång toorevert toohello ursprungliga ordning returneras av frågan.

Använd hello filtrera ikonen toonarrow sökningen.

![Sortera och filtrera kolumner](./media/app-insights-analytics-using/040.png)

### <a name="group-items"></a>Gruppera objekt
toosort använda gruppering av mer än en kolumn. Först aktivera den och dra kolumnrubrikerna till hello utrymme ovanför hello tabell.

![Grupp](./media/app-insights-analytics-using/060.png)

### <a name="missing-some-results"></a>Saknar vissa resultat?

Om du tror att det inte uppstår alla hello resultat som du förväntade dig, finns det några möjliga orsaker.

* **Intervallet tidsfiltret**. Som standard endast visas resultaten från hello senaste 24 timmarna. Det finns en automatisk filter som begränsar hello olika resultat som hämtas från hello källtabellerna. 

    Du kan dock ändra hello tidsintervall filtret med hjälp av hello nedrullningsbara menyn.

    Eller du kan åsidosätta automatisk hello-intervall genom att inkludera egna [ `where  ... timestamp ...` satsen](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) i frågan. Exempel:

    `requests | where timestamp > ago('2d')`

* **Resultaten gränsen**. Det finns en gräns på cirka 10 k rader på hello resultaten som returnerades från hello-portalen. En varning visas om du går över hello-gränsen. Om detta händer visar sortering av resultaten i hello tabellen inte alltid alla hello faktiska första eller sista resultat. 

    Det är god sed tooavoid hitting hello gränsen. Använda hello tidsfiltret intervallet, eller använda operatorer som:

  * [de 100 främsta av tidsstämpel](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) 
  * [ta 100](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html)
  * [Sammanfatta](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) 
  * [där tidsstämpel > ago(3d)](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html)

(Vill ha mer än 10 k raderna? Överväg att använda [löpande Export](app-insights-export-telemetry.md) i stället. Analytics är utformad för analys, i stället för hämtning rådata.)

## <a name="diagrams"></a>Diagram
Välj hello typ av diagram som du vill:

![Välj en diagramtyp av](./media/app-insights-analytics-using/230.png)

Om du har flera kolumner med rätt hello-typer kan välja du hello x och y-axlarna och en kolumn med dimensioner toosplit hello resultaten efter.

Som standard visas först som en tabell och du väljer hello diagram manuellt. Men du kan använda hello [återge direktiv](https://docs.loganalytics.io/queryLanguage/query_language_renderoperator.html) hello slutet av en fråga tooselect ett diagram.

### <a name="analytics-diagnostics"></a>Analytics-diagnostik


Om det finns en plötslig insamling eller steg i dina data på en timechart kan du se en markerad punkt hello rad. Detta anger att Analytics-diagnostik har identifierats av en kombination av egenskaper som filtrerar ut hello plötslig förändring. Klicka på hello punkt tooget mer information om hello filter och toosee hello filtrerad version. Detta kan hjälpa dig att identifiera vilken orsakade hello ändring. 

[Mer information om Analytics diagnostik](app-insights-analytics-diagnostics.md)


![Analytics-diagnostik](./media/app-insights-analytics-using/analytics-diagnostics.png)

## <a name="pin-toodashboard"></a>PIN-kod toodashboard
Du kan fästa en diagrammet eller tooone av din [delade instrumentpaneler](app-insights-dashboards.md) -Klicka bara på hello PIN-kod. (Du kan behöva för[uppgradera din app prissättning](app-insights-pricing.md) tooturn på den här funktionen.) 

![Klicka på hello PIN-kod](./media/app-insights-analytics-using/pin-01.png)

Det innebär att när du sätter ihop en instrumentpanel toohelp du övervaka hello prestanda och användning av web services, kan du inkludera ganska komplex analys tillsammans med hello andra mått. 

Du kan fästa en tabell toohello instrumentpanel, om den har fyra eller färre kolumner. Endast hello översta sju rader visas.

### <a name="dashboard-refresh"></a>Uppdatera instrumentpanelen
hello diagram Fäst toohello instrumentpanelen uppdateras automatiskt genom att köra frågan hello cirka varje timme. Du kan också klicka på uppdateringsknappen hello.

### <a name="automatic-simplifications"></a>Automatisk förenklingar

Vissa förenklingar är tillämpade tooa diagrammet när du fäster tooa instrumentpanelen.

**Tid begränsning:** frågorna är automatiskt begränsad toohello senaste 14 dagarna. hello effekt är hello samma som om frågan innehåller `where timestamp > ago(14d)`.

**Bin antal begränsning:** om du visar ett diagram med mycket diskreta lagerplatser (vanligtvis ett stapeldiagram) hello mindre ifyllda lagerplatser grupperas automatiskt i en enda ”andra” bin. Till exempel den här frågan:

    requests | summarize count_search = count() by client_CountryOrRegion

ser ut så här i Analytics:

![Diagram med lång pilslut](./media/app-insights-analytics-using/pin-07.png)

men när du fäster den tooa instrumentpanelen, det ser ut så här:

![Diagram med begränsad lagerplatser](./media/app-insights-analytics-using/pin-08.png)

## <a name="export-tooexcel"></a>Exportera tooExcel
När du har kört en fråga, kan du hämta en CSV-fil. Klicka på **Exportera Excel**.

## <a name="export-toopower-bi"></a>Exportera tooPower BI
Placera markören hello i en fråga och välj **exportera Power BI**.

![Exportera från Analytics tooPower BI](./media/app-insights-analytics-using/240.png)

Du kan köra hello frågan i Power BI. Du kan ange den toorefresh enligt ett schema.

Du kan skapa instrumentpaneler som samordnar data från olika källor med Power BI.

[Mer information om export tooPower BI](app-insights-export-power-bi.md)

## <a name="deep-link"></a>Djuplänk

Hämta en länk under **Export resursen länken** som du kan skicka tooanother användaren. Från hello användaren har [åtkomst tooyour resursgruppen](app-insights-resources-roles-access-control.md), hello frågan öppnas i hello Analytics Användargränssnittet.

(I hello länk hello Frågetexten visas efter ”? q =”, gzip komprimeras och Base64-kodad. Du kan skriva kod toogenerate djuplänkar du ange toousers. Hello bör dock sätt toorun Analytics från kod är med hjälp av hello [REST API](https://dev.applicationinsights.io/).)


## <a name="automation"></a>Automation

Använd hello [Data Access REST API: et](https://dev.applicationinsights.io/) toorun Analytics-frågor. [Till exempel](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (med PowerShell):

```PS
curl "https://api.applicationinsights.io/beta/apps/DEMO_APP/query?query=requests%7C%20where%20timestamp%20%3E%3D%20ago(24h)%7C%20count" -H "x-api-key: DEMO_KEY"
```

Till skillnad från hello Analytics UI, hello REST-API lägger inte automatiskt till tidsstämpel begränsning tooyour frågor. Kom ihåg tooadd egna where-satsen, tooavoid få stora svar.



## <a name="import-data"></a>Importera data

Du kan importera data från en CSV-fil. En typisk användning är tooimport statiska data som du kan ansluta med tabeller från din telemetri. 

Om autentiserade användare identifieras i din telemetri av ett alias eller dolda id, kan du importera en tabell som mappar alias tooreal namn. Genom att utföra en koppling på hello begärandetelemetri kan identifiera du användare efter verkliga namn i hello Analytics-rapporter.

### <a name="define-your-data-schema"></a>Definiera dataschemat

1. Klicka på **inställningar** (längst upp till vänster) och sedan **datakällor**. 
2. Lägg till en datakälla hello instruktionerna. Du tillfrågas toosupply ett exempel på hello data som ska innehålla minst tio rader. Du kan sedan Korrigera hello schemat.

Detta definierar en datakälla som du kan sedan använda tooimport enskilda tabeller.

### <a name="import-a-table"></a>Importera en tabell

1. Öppna din definitionen av datakällan hello-listan.
2. Klicka på ”ladda upp” och följ hello instruktioner tooupload hello tabell. Detta innebär en anropet tooa REST-API och det är därför enkelt tooautomate. 

Din tabell är nu tillgänglig för användning i Analytics-frågor. Den visas i Analytics 

### <a name="use-hello-table"></a>Använd hello tabell

Anta att dina definitionen av datakällan anropas `usermap`, och att det finns två fält `realName` och `user_AuthenticatedId`. Hej `requests` tabellen har också ett fält med namnet `user_AuthenticatedId`, så det är enkelt toojoin dem:

```AIQL

    requests
    | where notempty(user_AuthenticatedId) | take 10
    | join kind=leftouter ( usermap ) on user_AuthenticatedId 
```
hello resulterande tabellen för förfrågningar har en ytterligare kolumn `realName`.

### <a name="import-from-logstash"></a>Importera från LogStash

Om du använder [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), du kan använda Analytics tooquery loggarna. Använd hello [plugin-program som kommer data i Analytics](https://github.com/Microsoft/logstash-output-application-insights). 

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

