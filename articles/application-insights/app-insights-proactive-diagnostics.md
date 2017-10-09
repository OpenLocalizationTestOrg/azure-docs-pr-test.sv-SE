---
title: aaaSmart identifiering i Azure Application Insights | Microsoft Docs
description: "Application Insights utför automatisk djupgående analys av din app telemetri och varnar dig om potentiella problem."
services: application-insights
documentationcenter: windows
author: rakefetj
manager: carmonm
ms.assetid: 2eeb4a35-c7a1-49f7-9b68-4f4b860938b2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: bwren
ms.openlocfilehash: f794476088fc69154eda2077b7a5cdc769fab3a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="smart-detection-in-application-insights"></a>Smart identifiering i Application Insights
 Smart identifiering varnar automatiskt dig för eventuella prestandaproblem i ditt webbprogram. Utför proaktiv analys av hello telemetri som appen skickar för[Programinsikter](app-insights-overview.md). Om det finns en plötslig ökning av fel eller onormala mönster i klient- eller prestanda, får du en avisering. Den här funktionen krävs ingen konfiguration. Den fungerar om programmet skickar tillräckligt med telemetri.

Du kan komma åt varningar Smart av både från hello e-postmeddelanden visas, och hello Smart identifiering bladet.

## <a name="review-your-smart-detections"></a>Granska din Smart identifieringar
Du kan identifiera identifieringar på två sätt:

* **Du får ett e-postmeddelande** från Application Insights. Här är ett typexempel:
  
    ![E-postavisering](./media/app-insights-proactive-diagnostics/03.png)
  
    Klicka på hello stor knapp tooopen detalj i hello-portalen.
* **hello Smart identifiering panelen** på din app översikt bladet visar antalet nya aviseringar. Klicka på hello panelen toosee en lista över senaste aviseringarna.

![Visa senaste identifieringar](./media/app-insights-proactive-diagnostics/04.png)

Välj en avisering toosee information.

## <a name="what-problems-are-detected"></a>Vilka problem har identifierats?
Det finns tre typer av identifiering:

* [Smartkort identifiering - fel avvikelser](app-insights-proactive-failure-diagnostics.md). Vi använder maskininlärning tooset hello förväntat antal misslyckade begäranden för din app korrelerar med belastning och andra faktorer. Om hello felintervall går utanför hello förväntade kuvert kan skicka vi en avisering.
* [Smartkort identifiering - Prestandaavvikelser](app-insights-proactive-performance-diagnostics.md). Du får meddelanden om svarstid för en åtgärd eller beroende varaktighet långsammare jämfört med toohistorical baslinjen eller om vi identifiera ett avvikande mönster i svarstid eller sidinläsningstiden.   
* [Smartkort identifiering - problem med Azure Cloud Service](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/). Du får aviseringar om din app finns i Azure Cloud Services och en rollinstans har startfel, ofta återvinning eller runtime kraschar.

(hello hjälplänkar i varje meddelande tar du toohello relevanta artiklar.)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Nästa steg
Dessa verktyg för Nätverksdiagnostik hjälpa dig att inspektera hello telemetri från din app:

* [Mått explorer](app-insights-metrics-explorer.md)
* [Sök explorer](app-insights-diagnostic-search.md)
* [Analytics - frågespråket kraftfulla](app-insights-analytics-tour.md)

Smart identifiering är helt automatisk. Men du kanske vill tooset in några fler aviseringar?

* [Manuellt konfigurerade mått aviseringar](app-insights-alerts.md)
* [Tillgänglighetstester för webbprogram](app-insights-monitor-web-app-availability.md) 

