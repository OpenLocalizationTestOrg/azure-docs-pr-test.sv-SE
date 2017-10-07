---
title: aaaDebug program med Azure Application Insights i Visual Studio | Microsoft Docs
description: "Prestandaanalys och diagnostik för webbappar vid felsökning och i produktion."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 2059802b-1131-477e-a7b4-5f70fb53f974
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/7/2017
ms.author: bwren
ms.openlocfilehash: 20491fbe4505bf719039e5d1c220b1afec01db25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-applications-with-azure-application-insights-in-visual-studio"></a>Felsöka program med Azure Application Insights i Visual Studio
I Visual Studio (2015 och senare) kan du analysera prestanda och diagnostisera problem i din ASP.NET-webbapp både när du felsöker och i produktion med hjälp av telemetri från [Azure Application Insights](app-insights-overview.md).

Om du har skapat ditt ASP.NET-webbprogram med hjälp av Visual Studio 2017 eller senare, den redan har hello Application Insights SDK. Annars, om du inte redan gjort det, [Lägg till Application Insights tooyour app](app-insights-asp-net.md).

toomonitor din app när den är i live produktion du normalt visar hello Application Insights telemetri i hello [Azure-portalen](https://portal.azure.com), där du kan ställa in aviseringar och använda kraftfulla övervakningsverktyg. Men för felsökning kan du också söka efter och analysera hello telemetri i Visual Studio. Du kan använda Visual Studio tooanalyze telemetri både från webbplatsen produktion och felsökning körs på utvecklingsdatorn. Du kan analysera felsökning körs även om du ännu inte har konfigurerat hello SDK toosend telemetri toohello Azure-portalen i hello senare fallet. 

## <a name="run"></a> Felsöka ditt projekt
Kör din webbapp i lokalt felsökningsläge genom att välja F5. Öppna olika sidor toogenerate vissa telemetri.

I Visual Studio kan du se antalet hello-händelser som har loggats av hello Application Insights-modulen i projektet.

![I Visual Studio visar hello Application Insights knappen vid felsökning.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

Klicka på den här knappen toosearch din telemetri. 

## <a name="application-insights-search"></a>Söka med Application Insights
hello Application Insights sökfönstret visar händelser som har loggats. (Om du har loggat in tooAzure när du ställer in Application Insights, kan du söka hello samma händelser i hello Azure-portalen.)

![Högerklicka på hello-projektet och välj Application Insights sökning](./media/app-insights-visual-studio/34.png)

> [!NOTE] 
> När du markera eller avmarkera filter klickar du på hello sökknappen hello slutet av hello textfält för sökning.
>

hello ledigt textsökning fungerar på alla fält i hello händelser. Till exempel söka efter en del av hello-URL för en sida. eller hello-värdet för en egenskap som klienten ort; eller specifika ord i en spårningslogg.

Klicka på någon händelse toosee detaljerade egenskaper.

Du kan klicka på via toohello kod för begäranden tooyour webbapp.

![Klicka på via toohello kod under begär information](./media/app-insights-visual-studio/31.png)

Du kan också öppna relaterade objekt toohelp diagnostisera misslyckade begäranden och undantag.

![Rulla nedåt toorelated objekt under begär information](./media/app-insights-visual-studio/41.png)

## <a name="view-exceptions-and-failed-requests"></a>Visa undantag och misslyckade begäranden
Undantag rapporter visas i fönstret för hello Sök. (I vissa äldre typer av ASP.NET-program kan ha för[konfigurera undantagsövervakning](app-insights-asp-net-exceptions.md) toosee undantag som hanteras av hello framework.)

Klicka på ett undantag tooget stack-spårning. Du kan klicka på via från hello stack trace toohello relevanta kodrad hello om hello koden för hello app är öppna i Visual Studio.

![Undantag för stackspårning](./media/app-insights-visual-studio/17.png)

## <a name="view-request-and-exception-summaries-in-hello-code"></a>Visa sammanfattningar begäran och undantag i hello kod
I hello kod lins rad ovanför varje hanterarmetoden visas antalet hello begäranden och undantag som loggats av Application Insights i hello föregående 24 timmar.

![Undantag för stackspårning](./media/app-insights-visual-studio/21.png)

> [!NOTE] 
> Koden lins visar Application Insights-data om du har [konfigurerat Application Insights-portalen app toosend telemetri toohello](app-insights-asp-net.md).
>

[Läs mer om Application Insights i CodeLens](app-insights-visual-studio-codelens.md)

## <a name="trends"></a>Trends
Trends är ett verktyg för visualisering av hur din app ska bete sig över tiden. 

Välj **utforska telemetri trender** från hello Application Insights verktygsfältsknappen eller Application Insights sökfönstret. Välj en av fem vanliga frågor tooget igång. Du kan analysera olika datauppsättningar baserat på telemetrityper, tidsintervall och andra egenskaper. 

toofind avvikelser i dina data, välja en av hello avvikelseidentifiering alternativ under hello ”vytypen” listrutan. hello filtreringsalternativ längst hello hello fönstret gör det enkelt toohone i på specifika delmängder av dina telemetri.

![Trends](./media/app-insights-visual-studio/51.png)

[Mer om Trends](app-insights-visual-studio-trends.md).

## <a name="local-monitoring"></a>Lokal övervakning
(Från Visual Studio 2015 Update 2) Om du inte har konfigurerat hello SDK toosend telemetri toohello Application Insights-portalen (så att det finns ingen instrumentation nyckel i ApplicationInsights.config) visar hello diagnostics-fönstret telemetri från din senaste felsökningssessionen. 

Detta är lämpligt om du redan har publicerat en tidigare version av din app. Vill du inte hello telemetri från dina felsökning sessioner toobe blandas med hello telemetri för hello Application Insights-portalen från hello publicerade app.

Det är också användbart om du har några [telemetri om anpassade](app-insights-api-custom-events-metrics.md) som du vill toodebug innan du skickar telemetri toohello portal.

* *Först ska konfigurerade jag fullständigt Application Insights toosend telemetri toohello-portalen. Men nu vill jag toosee hello telemetri endast i Visual Studio.*
  
  * I fönstret hello-Sök inställningar finns en alternativet toosearch lokala diagnostik även om din app skicka telemetri toohello portal.
  * toostop telemetri skickas toohello portal, kommentera hello rad `<instrumentationkey>...` från ApplicationInsights.config. När du är klar toosend telemetri toohello portal kan Avkommentera den.


## <a name="next-steps"></a>Nästa steg
|  |  |
| --- | --- |
| **[Lägga till mer information](app-insights-asp-net-more.md)**<br/>Övervaka användning, tillgänglighet, beroenden och undantag. Integrera spårningar från loggningsramverk. Skriv anpassad telemetri. |![Visual Studio](./media/app-insights-visual-studio/64.png) |
| **[Arbeta med hello Application Insights-portalen](app-insights-dashboards.md)**<br/>Visa instrumentpaneler, kraftfulla verktyg för diagnostik- och analytiska, aviseringar, en levande beroendekarta för anslutningar av programmet och exporterade telemetridata. |![Visual Studio](./media/app-insights-visual-studio/62.png) |

