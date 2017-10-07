---
title: "aaaAzure Programinsikter för ASP.NET Core | Microsoft Docs"
description: "Övervaka webbprogram för tillgänglighet, prestanda och användning."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: a7a27f9eef1daec5b0deae9fd88906e646980659
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights för ASP.NET Core
[Application Insights](app-insights-overview.md) kan du övervaka ditt webbprogram för tillgänglighet, prestanda och användning. Du kan göra välgrundade beslut om hello riktning hello design med hello feedback du använda om hello prestanda och effektivitet för din app i hello, i varje utvecklingslivscykeln för.

![Exempel](./media/app-insights-asp-net-core/sample.png)

Du behöver en prenumeration med [Microsoft Azure](http://azure.com). Logga in med ett Microsoft-konto som du kanske har för Windows, XBox Live eller andra Microsoft-molntjänster. Gruppen kan ha en organisationens prenumeration tooAzure: Be hello ägare tooadd tooit med ditt Microsoft-konto.

## <a name="getting-started"></a>Komma igång

* Högerklicka på ditt projekt i Visual Studio Solution Explorer och markera **konfigurera Application Insights**, eller **Lägg till > Application Insights**. [Läs mer](app-insights-asp-net.md).
* Om du inte ser dessa kommandon på menyn följer hello [manuell komma igång-guide](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started). Du kan behöva toodo detta om projektet har skapats med en version av Visual Studio innan 2017.

## <a name="using-application-insights"></a>Använda Application Insights
Logga in på hello [Microsoft Azure-portalen](https://portal.azure.com)väljer **alla resurser** eller **Programinsikter**, och välj sedan hello resurs du skapat toomonitor din app.

Använda din app på ett tag i en separat webbläsarfönster. Data som visas i hello Application Insights diagram visas. (Du kanske tooclick uppdatering.) Det är bara en liten mängd data medan du utvecklar, men dessa diagram verkligen kan underlätta alive när du publicerar din app och har många användare. 

hello översiktssidan visar viktiga prestandadiagram: serversvarstid, sidinläsningstiden och antalet misslyckade begäranden. Klicka på alla diagram toosee flera diagram och data.

Vyer i hello portal delas in i tre huvudkategorier:

* [Metrics Explorer](app-insights-metrics-explorer.md) visar diagram och tabeller för mått och antal, till exempel svarstider, fel eller mått som du skapar själv med hello [API](app-insights-api-custom-events-metrics.md). Filtrera och segment hello data genom att egenskapen värden tooget en bättre förståelse för din app och dess användare.
* [Sök Explorer](app-insights-diagnostic-search.md) visar enskilda händelser, till exempel specifika begäranden, undantag, loggspårningar eller händelser som du själv har skapat med hello [API](app-insights-api-custom-events-metrics.md). Filtrera och söka i hello händelser och navigera mellan relaterade händelser tooinvestigate problem.
* [Analytics](app-insights-analytics.md) kan du köra SQL-liknande frågor via din telemetri och är ett kraftfullt verktyg för analys och diagnostik.

## <a name="alerts"></a>Aviseringar
* Automatiskt få [proaktiv diagnostiska aviseringar](app-insights-proactive-diagnostics.md) som berättar om avvikande ändringar i fel och andra mått.
* Ställ in [tillgänglighetstester](app-insights-monitor-web-app-availability.md) tootest webbplatsen kontinuerligt från platser över hela världen och få e-postmeddelanden när någon testa misslyckas.
* Ställ in [mått aviseringar](app-insights-monitor-web-app-availability.md) tooknow om mått som svarstider eller undantag priser går utanför acceptabla gränser.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="open-source"></a>Öppen källkod
[Läsa och bidra toohello kod](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)


## <a name="next-steps"></a>Nästa steg
* [Lägga till telemetri tooyour webbsidor](app-insights-javascript.md) toomonitor sidan användnings- och prestandadata.
* [Övervaka beroenden](app-insights-asp-net-dependencies.md) toosee om REST, SQL eller andra externa resurser saktar ner dig.
* [Använd hello API](app-insights-api-custom-events-metrics.md) toosend egna händelser och mått för en mer detaljerad vy av appens prestanda och användning.
* [Tillgänglighetstester](app-insights-monitor-web-app-availability.md) Kontrollera din app ständigt hello världen. 

