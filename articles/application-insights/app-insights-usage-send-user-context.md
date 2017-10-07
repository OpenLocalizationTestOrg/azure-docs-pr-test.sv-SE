---
title: "aaaSending användaren kontexten tooenable användning inträffar i Azure Application Insights | Microsoft Docs"
description: "Spåra hur användarna flyttar din tjänst när du tilldelar dem en unik, beständig ID-sträng i Application Insights."
services: application-insights
documentationcenter: 
author: abgreg
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: csharp
ms.topic: article
ms.date: 08/02/2017
ms.author: bwren
ms.openlocfilehash: 0e6c2348f53a3ea970060334179b0dd070925e82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#  <a name="sending-user-context-tooenable-usage-experiences-in-azure-application-insights"></a>Skicka användare kontexten tooenable användning inträffar i Azure Application Insights

## <a name="tracking-users"></a>Spåra användare

Application Insights aktiverar du toomonitor och spåra dina användare via en uppsättning av verktyg för användning av produkten: 
* [Användare, sessioner, händelser](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [Trattar](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [Kvarhållning](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* Kohorter
* [Arbetsböcker](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

I ordning tootrack vad användaren har över tid, måste ett ID för varje användare eller session i Application Insights. Inkludera dessa ID: N i varje vy händelse eller sidan.
- Användare, skorstenar, kvarhållning och kohort: inkludera användar-ID.
- Sessioner: Inkludera sessions-ID.

Om din app är integrerat med hello [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), user ID spåras automatiskt.

## <a name="choosing-user-ids"></a>Om du väljer användar-ID

Användar-ID ska spara över användaren sessioner tootrack funktionssättet för användare över tid. Det finns olika sätt att spara hello-ID.
- En definition av en användare som du redan har i din tjänst.
- Om hello-tjänsten har åtkomst tooa webbläsare, kan skickas hello webbläsaren en cookie med ID i den. hello ID behålls för länge hello cookie i hello användarens webbläsare.
- Om det behövs kan du använda ett nytt ID varje session, men hello resultat om användarna kommer att vara begränsad. Till exempel inte kan toosee hur en användarbeteende ändras med tiden.

hello-ID ska vara ett Guid eller en annan sträng tillräckligt komplext tooidentify varje användare unikt. Exempelvis kan det vara ett långt slumptal.

Om hello ID innehåller personlig information om hello användaren kan är den inte ett lämpligt värde toosend tooApplication insikter som ett användar-ID. Du kan skicka sådan ID som en [autentiserat användar-ID](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), men den uppfyller inte hello användar-ID krav för Användningsscenarier.

## <a name="aspnet-apps-set-user-context-in-an-itelemetryinitializer"></a>ASP.NET appar: Ange användarkontext i en ITelemetryInitializer

Skapa en telemetri initieraren som beskrivs i detalj [här](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer), och ange hello Context.User.Id och hello Context.Session.Id.

Det här exemplet anger hello användar-ID tooan ID som upphör att gälla efter hello-sessionen. Använd om möjligt ett användar-ID som kvarstår mellan sessioner.

*C#*

```C#

    using System;
    using System.Web;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that sets hello user ID.
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            // For a full experience, track each user across sessions. For an incomplete view of user 
            // behavior within a session, store user ID on hello HttpContext Session.
            // Set hello user ID if we haven't done so yet.
            if (HttpContext.Current.Session["UserId"] == null)
            {
                HttpContext.Current.Session["UserId"] = Guid.NewGuid();
            }

            // Set hello user id on hello Application Insights telemetry item.
            telemetry.Context.User.Id = (string)HttpContext.Current.Session["UserId"];

            // Set hello session id on hello Application Insights telemetry item.
            telemetry.Context.Session.Id = HttpContext.Current.Session.SessionID;
        }
      }
    }
```

## <a name="next-steps"></a>Nästa steg
- tooenable användning inträffar, börja skicka [anpassade händelser](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) eller [sidvisningar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Om du redan skickar anpassade händelser eller sidvyer utforska hello användning verktyg toolearn hur användare att använda tjänsten.
    * [Översikt över användning](app-insights-usage-overview.md)
    * [Användare, sessioner och händelser](app-insights-usage-segmentation.md)
    * [Trattar](usage-funnels.md)
    * [Kvarhållning](app-insights-usage-retention.md)
    * [Arbetsböcker](app-insights-usage-workbooks.md)
