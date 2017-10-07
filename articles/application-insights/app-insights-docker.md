---
title: aaaMonitor Docker-program i Azure Application Insights | Microsoft Docs
description: "Docker prestandaräknarna, händelser och undantag kan visas i Application Insights, tillsammans med hello telemetri från hello av appar."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 27a3083d-d67f-4a07-8f3c-4edb65a0a685
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 9aaf1076bae25485a396db1bb3dcd13bccd87c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-docker-applications-in-application-insights"></a>Övervaka Docker-program i Application Insights
Livscykelhändelser och prestanda prestandaräknare från [Docker](https://www.docker.com/) kan ritas upp behållare på Application Insights. Installera hello [Programinsikter](app-insights-overview.md) bilden i en behållare i värden och den visar prestandaräknare för hello värden som hello bra som för andra bilder.

Med Docker, kan du distribuera dina appar i lightweight-behållare med alla beroenden. De kan köras på en värddator som kör en Docker-motorn.

När du kör hello [Application Insights bild](https://hub.docker.com/r/microsoft/applicationinsights/) på Docker-värden som du får följande fördelar:

* Livscykel telemetri om alla hello-behållare som körs på värd hello - starta, stoppa och så vidare.
* Prestandaräknare för alla hello-behållare. CPU, minne, nätverksanvändning och mer.
* Om du [installerat Application Insights SDK för Java](app-insights-java-live.md) i hello-appar som körs i hello behållare, alla hello telemetri för dessa appar har ytterligare egenskaper som identifierar hello-behållaren och värddatorn. Om du har instanser av en app som körs i mer än en värddator kan du exempelvis enkelt filtrera din app telemetri av värden.

![Exempel](./media/app-insights-docker/00.png)

## <a name="set-up-your-application-insights-resource"></a>Konfigurera Application Insights-resurs
1. Logga in på [Microsoft Azure-portalen](https://azure.com) och öppna hello Application Insights-resurs för din app, eller [skapa en ny](app-insights-create-new-resource.md). 
   
    *Vilken resurs som ska jag använda?* Om hello-appar som körs på värden har utvecklats av någon annan, måste du för[skapar en ny Application Insights-resurs](app-insights-create-new-resource.md). Detta är där du kan visa och analysera hello telemetri. (Välj Allmänt för typ av hello app).
   
    Men om du utvecklar hello hello appar sedan vi hoppas att du [lagt till Application Insights SDK](app-insights-java-live.md) tooeach av dem. Du kan konfigurera alla toosend telemetri tooone resurs och ska du använda samma resurs toodisplay hello Docker livscykel och prestanda information om de är alla verkligen komponenter i ett enda affärsprogram. 
   
    Ett tredje scenario är att du har utvecklat de flesta av hello appar, men du använder separata resurser toodisplay sina telemetri. I så fall måste du förmodligen också vill toocreate en separat resurs för hello Docker-data. 
2. Lägg till hello Docker panelen: Välj **lägga till panelen**dra hello Docker sida vid sida från hello-galleriet och klicka sedan på **klar**. 
   
    ![Exempel](./media/app-insights-docker/03.png)
3. Klicka på hello **Essentials** listrutan och kopiera hello Instrumentation nyckel. Du använder den här tootell hello SDK där toosend dess telemetri.

    ![Exempel](./media/app-insights-docker/02-props.png)

Hålla webbläsarfönstret praktiska, som du återkommer tooit snart toolook din telemetri.

## <a name="run-hello-application-insights-monitor-on-your-host"></a>Kör hello Application Insights monitor på din värd
Nu när du har någonstans toodisplay hello telemetri kan ställa du in hello av app som samlar in och skicka den.

1. Anslut tooyour Docker-värden. 
2. Redigera din instrumentation nyckeln i det här kommandot och kör det:
   
   ```
   
   docker run -v /var/run/docker.sock:/docker.sock -d microsoft/applicationinsights ikey=000000-1111-2222-3333-444444444
   ```

Endast en Application Insights-avbildning krävs per Docker-värd. Om programmet distribueras på flera Docker-värdar, upprepa sedan hello-kommando på varje värd.

## <a name="update-your-app"></a>Uppdatera ditt program
Om ditt program är försett med hello [Application Insights SDK för Java](app-insights-java-get-started.md), Lägg till följande rad i hello ApplicationInsights.xml-filen i projektet, under hello hello `<TelemetryInitializers>` element:

```xml

    <Add type="com.microsoft.applicationinsights.extensibility.initializer.docker.DockerContextInitializer"/> 
```

Detta lägger till Docker information, till exempel behållare och värd-id tooevery telemetri objekt som skickats från din app.

## <a name="view-your-telemetry"></a>Visa telemetrin
Gå tillbaka tooyour Application Insights-resurs i hello Azure-portalen.

Klicka dig igenom hello Docker-panelen.

Kort visas data som kommer från hello Docker-app, särskilt om du har andra behållare som körs på Docker-motorn.

Här är några av hello vyer som du kan hämta.

### <a name="perf-counters-by-host-activity-by-image"></a>Prestandaräknarna av värden, aktivitet efter bilden
![Exempel](./media/app-insights-docker/10.png)

![Exempel](./media/app-insights-docker/11.png)

Klicka på värden eller image-namn för mer information.

toocustomize hello vyn klickar du på alla diagram, hello rutnätet rubrik eller Använd Lägg till diagram. 

[Mer information om metrics explorer](app-insights-metrics-explorer.md).

### <a name="docker-container-events"></a>Docker behållaren händelser
![Exempel](./media/app-insights-docker/13.png)

tooinvestigate enskilda händelser, klickar du på [Sök](app-insights-diagnostic-search.md). Sök och filtrera toofind hello händelser som du vill. Klicka på en händelse tooget detalj.

### <a name="exceptions-by-container-name"></a>Undantag av behållarens namn
![Exempel](./media/app-insights-docker/14.png)

### <a name="docker-context-added-tooapp-telemetry"></a>Docker-kontexten läggs tooapp telemetri
Telemetri som skickas från hello programmet försett med AI SDK, utökat med Docker kontext för begäran:

![Exempel](./media/app-insights-docker/16.png)

Processortid och tillgängligt minnesprestandaräknare utökat grupperade efter Docker behållarnamn:

![Exempel](./media/app-insights-docker/15.png)

## <a name="q--a"></a>Frågor och svar
*Vad Application Insights mig som jag inte kan hämta från Docker?*

* Specificering av prestandaräknare av behållaren och image.
* Integrera behållare och app-data i en instrumentpanel.
* [Exportera telemetri](app-insights-export-telemetry.md) för ytterligare analys tooa databasen, Power BI eller andra instrumentpanelen.

*Hur får jag telemetri från själva hello appen?*

* Installera hello Application Insights SDK i hello app. Lär dig hur för: [Java-webbappar](app-insights-java-get-started.md), [Windows web apps](app-insights-asp-net.md).

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Nästa steg

* [Application Insights för Java](app-insights-java-get-started.md)
* [Application Insights för Node.js](app-insights-nodejs.md)
* [Application Insights för ASP.NET](app-insights-asp-net.md)
