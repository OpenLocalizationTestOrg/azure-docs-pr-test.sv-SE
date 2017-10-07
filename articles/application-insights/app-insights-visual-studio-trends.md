---
title: aaaAnalyzing trender i Visual Studio | Microsoft Docs
description: Analysera, visualisera och utforska trender i Application Insights Telemetry i Visual Studio.
services: application-insights
documentationcenter: .net
author: numberbycolors
manager: carmonm
ms.assetid: 3150c6fc-2691-44f6-a290-fc5cd68e692a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: bwren
ms.openlocfilehash: 5c623ec040363f05e80ca927dc8855eb016adc99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyzing-trends-in-visual-studio"></a>Analysera trender i Visual Studio
hello Application Insights trender verktyget visualizes hur ditt webbprogram viktiga telemetriska händelser ändras över tiden, vilket hjälper dig att snabbt identifiera problem och avvikelser. Detaljerad diagnostisk information genom att länka toomore trender kan hjälpa dig att förbättra prestandan för din app, spåra hello orsakerna till undantag och få insikter från dina anpassade händelser.

![Exempel på fönster i Trends](./media/app-insights-visual-studio-trends/app-insights-trends-hero-750.png)

## <a name="configure-your-web-app-for-application-insights"></a>Konfigurera webbappen för Application Insights

Om du inte redan har gjort det ska du [konfigurera webbappen för Application Insights](app-insights-overview.md). Detta gör att den toosend telemetri toohello Application Insights-portalen. hello trender verktyget läser hello telemetri därifrån.

Application Insights Trends är tillgängligt i Visual Studio 2015 Update 3 och senare.

## <a name="open-application-insights-trends"></a>Öppna Application Insights Trends
tooopen hello Application Insights trender fönster:

* Knapp för hello Application Insights, Välj **utforska telemetri trender**, eller
* Snabbmenyn hello projektet, Välj **Programinsikter > utforska telemetri trender**, eller
* Hello menyraden för Visual Studio, Välj **Visa > andra fönster > Application Insights trender**.

Du kan se en fråga tooselect en resurs. Klicka på **väljer du en resurs**, logga in med en Azure-prenumeration och välj sedan en Application Insights-resurs hello listan som du vill att tooanalyze telemetri trender.

## <a name="choose-a-trend-analysis"></a>Välj en trendanalys
![Meny över vanliga typer av trendanalyser](./media/app-insights-visual-studio-trends/app-insights-trends-1-750.png)

Kom igång genom att välja en av fem vanliga trend analyser, varje när data från hello senaste 24 timmarna:

* **Undersöka prestandaproblem med din serverbegäranden** -begäranden som görs tooyour tjänsten, grupperade efter svarstider
* **Analysera fel i din serverbegäranden** -begäranden som görs tooyour tjänsten, grupperade efter HTTP-svarskoden
* **Granska hello undantag i ditt program** -undantag från tjänsten, grupperade efter undantagstyp
* **Kontrollera hello prestandan för din programberoenden** -tjänster som anropas av tjänsten, grupperade efter svarstider
* **Granska dina anpassade händelser** – Anpassade händelser som du har konfigurerat för din tjänst, grupperade efter händelsetyp.

Inbyggd analys finns dessa senare från hello **visa vanliga typer av telemetri analysis** i hello övre vänstra hörnet av hello trender-fönstret.

## <a name="visualize-trends-in-your-application"></a>Visualisera trender i ditt program
Application Insights Trends skapar en tidsserievisualisering från din apps telemetri. Varje tidsserievisualisering visar en typ av telemetri, grupperad efter en egenskap för den telemetrin, över en viss tidsperiod. Exempelvis kanske du vill tooview serverbegäranden grupperade efter hello land där de har sitt ursprung, över hello senaste 24 timmarna. I det här exemplet skulle varje bubbla på hello visualiseringen representerar antalet hello serverbegäranden för vissa land/region under en timme.

Använd hello styr hello överst i hello fönstret tooadjust vilka typer av du visa telemetri. Börja med att välja hello telemetri typer som du är intresserad av:

* **Typ av telemetri** – Serverbegäranden, undantag, beroenden eller anpassade händelser
* **Tidsintervall** - var som helst från hello senaste 30 minuterna toohello senaste tre dagarna
* **Gruppera efter** – Undantagstyp, problem-ID, land/region och mer.

Klicka på **analysera telemetri** toorun hello frågan.

toonavigate mellan bubblor i hello visualiseringen:

* Klicka på tooselect en bubbla som uppdaterar hello filter längst ned hello hello fönstret Sammanfattning bara hello händelser som inträffade under en viss tidsperiod
* Dubbelklicka på ett sökverktyg bubblan toonavigate toohello och se alla hello enskilda telemetriska händelser som inträffade under den tidsperioden
* CTRL-klicka bubblan toode – Välj den i hello visualiseringen.

> [!TIP]
> Hej trender och Sök verktyg tillsammans toohelp du hitta hello orsaken till problem i din tjänst mellan tusentals telemetriska händelser. Om t.ex. en av dina kunder en eftermiddag noterar att din app svarar sämre, kan du börja med Trends. Analysera begäranden tooyour tjänsten via hello föregående timmarna, grupperade efter svarstid. Se efter om det finns ett ovanligt stort antal långsamma begäranden. Dubbelklicka sedan på det bubblan toogo toohello sökverktyget, filtrerade toothose begäran händelser. Från sökning, du kan utforska hello innehållet i dessa begäranden och navigera toohello koden berörs tooresolve hello problemet.
> 
> 

## <a name="filter"></a>Filter
Identifiera mer specifika trender med hello filterkontroller längst hello hello-fönstret. tooapply ett filter, klicka på dess namn. Du kan växla mellan olika filter toodiscover trender som kan dölja i en viss dimension för din telemetri. Om du använder ett filter i en dimension som undantagstyp, förblir filter i andra dimensioner klickbara trots att de visas nedtonat. tooun-tillämpa ett filter, klicka på den igen. CTRL-klicka tooselect flera filter i hello samma dimension.

![Filter för trender](./media/app-insights-visual-studio-trends/TrendsFiltering-750.png)

Vad händer om du vill tooapply flera filter? 

1. Filtret hello första. 
2. Klicka på hello **tillämpa markerade filter och fråga igen** knappen av hello namnet på ditt första filter hello dimension. Detta kommer att fråga din telemetri för händelser som matchar hello första filtret. 
3. Tillämpa ett andra filter. 
4. Upprepa hello processen toofind trender i specifika delmängder av dina telemetri. Till exempel serverbegäranden med namnet ”GET Home/Index” *och* som kommit från Tyskland *och* som tagit emot svarskod 500. 

tooun-använda en av dessa filter: Klicka på hello **ta bort valda filter och fråga igen** knappen för hello dimensionen.

![Flera filter](./media/app-insights-visual-studio-trends/TrendsFiltering2-750.png)

## <a name="find-anomalies"></a>Identifiera avvikelser
hello trender verktyget kan markera bubblor över händelser som har avvikande jämfört med tooother bubblor i hello samma tid serien. Välj i listrutan vytypen hello **antal tidsenhet (Markera avvikelser)** eller **procenttal i tidsenhet (Markera avvikelser)**. Röda bubblor representerar bubblor med avvikande händelser. Avvikelser definieras som bubblor med antal/procenttal överstiger 2.1 gånger hello standardavvikelsen för hello antal/procenttal som uppstått i hello efter två tidsperioder (48 timmar om du visar hello senaste 24 timmarna, osv.).

![Färgade punkter representerar avvikelser](./media/app-insights-visual-studio-trends/TrendsAnomalies-750.png)

> [!TIP]
> Att markera avvikelser är till särskilt stor hjälp när man försöker hitta avvikande mätvärden i tidsserier med små bubblor som annars kan se ungefär lika stora ut.  
> 
> 

## <a name="next"></a>Nästa steg
|  |  |
| --- | --- |
| **[Arbeta med Application Insights i Visual Studio](app-insights-visual-studio.md)**<br/>Sök i telemetri, visa data i CodeLens och konfigurera Application Insights. Allt i Visual Studio. |![Högerklicka på hello-projektet och välj Application Insights sökning](./media/app-insights-visual-studio-trends/34.png) |
| **[Lägga till mer information](app-insights-asp-net-more.md)**<br/>Övervaka användning, tillgänglighet, beroenden och undantag. Integrera spårningar från loggningsramverk. Skriv anpassad telemetri. |![Visual Studio](./media/app-insights-visual-studio-trends/64.png) |
| **[Arbeta med hello Application Insights-portalen](app-insights-dashboards.md)**<br/>Instrumentpaneler, kraftfulla verktyg för diagnostik och analys, aviseringar, live-mappning över beroenden för din app och telemetriexport. |![Visual Studio](./media/app-insights-visual-studio-trends/62.png) |

