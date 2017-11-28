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
#  <a name="sending-user-context-tooenable-usage-experiences-in-azure-application-insights"></a><span data-ttu-id="2bbd6-103">Skicka användare kontexten tooenable användning inträffar i Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="2bbd6-103">Sending user context tooenable usage experiences in Azure Application Insights</span></span>

## <a name="tracking-users"></a><span data-ttu-id="2bbd6-104">Spåra användare</span><span class="sxs-lookup"><span data-stu-id="2bbd6-104">Tracking users</span></span>

<span data-ttu-id="2bbd6-105">Application Insights aktiverar du toomonitor och spåra dina användare via en uppsättning av verktyg för användning av produkten:</span><span class="sxs-lookup"><span data-stu-id="2bbd6-105">Application Insights enables you toomonitor and track your users through a set of product usage tools:</span></span> 
* [<span data-ttu-id="2bbd6-106">Användare, sessioner, händelser</span><span class="sxs-lookup"><span data-stu-id="2bbd6-106">Users, Sessions, Events</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [<span data-ttu-id="2bbd6-107">Trattar</span><span class="sxs-lookup"><span data-stu-id="2bbd6-107">Funnels</span></span>](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [<span data-ttu-id="2bbd6-108">Kvarhållning</span><span class="sxs-lookup"><span data-stu-id="2bbd6-108">Retention</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* <span data-ttu-id="2bbd6-109">Kohorter</span><span class="sxs-lookup"><span data-stu-id="2bbd6-109">Cohorts</span></span>
* [<span data-ttu-id="2bbd6-110">Arbetsböcker</span><span class="sxs-lookup"><span data-stu-id="2bbd6-110">Workbooks</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

<span data-ttu-id="2bbd6-111">I ordning tootrack vad användaren har över tid, måste ett ID för varje användare eller session i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-111">In order tootrack what a user does over time, Application Insights needs an ID for each user or session.</span></span> <span data-ttu-id="2bbd6-112">Inkludera dessa ID: N i varje vy händelse eller sidan.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-112">Include these IDs in every custom event or page view.</span></span>
- <span data-ttu-id="2bbd6-113">Användare, skorstenar, kvarhållning och kohort: inkludera användar-ID.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-113">Users, Funnels, Retention, and Cohorts: Include user ID.</span></span>
- <span data-ttu-id="2bbd6-114">Sessioner: Inkludera sessions-ID.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-114">Sessions: Include session ID.</span></span>

<span data-ttu-id="2bbd6-115">Om din app är integrerat med hello [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), user ID spåras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-115">If your app is integrated with hello [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), user ID is tracked automatically.</span></span>

## <a name="choosing-user-ids"></a><span data-ttu-id="2bbd6-116">Om du väljer användar-ID</span><span class="sxs-lookup"><span data-stu-id="2bbd6-116">Choosing user IDs</span></span>

<span data-ttu-id="2bbd6-117">Användar-ID ska spara över användaren sessioner tootrack funktionssättet för användare över tid.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-117">User IDs should persist across user sessions tootrack how users behave over time.</span></span> <span data-ttu-id="2bbd6-118">Det finns olika sätt att spara hello-ID.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-118">There are various approaches for persisting hello ID.</span></span>
- <span data-ttu-id="2bbd6-119">En definition av en användare som du redan har i din tjänst.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-119">A definition of a user that you already have in your service.</span></span>
- <span data-ttu-id="2bbd6-120">Om hello-tjänsten har åtkomst tooa webbläsare, kan skickas hello webbläsaren en cookie med ID i den.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-120">If hello service has access tooa browser, it can pass hello browser a cookie with an ID in it.</span></span> <span data-ttu-id="2bbd6-121">hello ID behålls för länge hello cookie i hello användarens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-121">hello ID will persist for as long as hello cookie remains in hello user's browser.</span></span>
- <span data-ttu-id="2bbd6-122">Om det behövs kan du använda ett nytt ID varje session, men hello resultat om användarna kommer att vara begränsad.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-122">If necessary, you can use a new ID each session, but hello results about users will be limited.</span></span> <span data-ttu-id="2bbd6-123">Till exempel inte kan toosee hur en användarbeteende ändras med tiden.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-123">For example, you won't be able toosee how a user's behavior changes over time.</span></span>

<span data-ttu-id="2bbd6-124">hello-ID ska vara ett Guid eller en annan sträng tillräckligt komplext tooidentify varje användare unikt.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-124">hello ID should be a Guid or another string complex enough tooidentify each user uniquely.</span></span> <span data-ttu-id="2bbd6-125">Exempelvis kan det vara ett långt slumptal.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-125">For example, it could be a long random number.</span></span>

<span data-ttu-id="2bbd6-126">Om hello ID innehåller personlig information om hello användaren kan är den inte ett lämpligt värde toosend tooApplication insikter som ett användar-ID.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-126">If hello ID contains personally identifying information about hello user, it is not an appropriate value toosend tooApplication Insights as a user ID.</span></span> <span data-ttu-id="2bbd6-127">Du kan skicka sådan ID som en [autentiserat användar-ID](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), men den uppfyller inte hello användar-ID krav för Användningsscenarier.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-127">You can send such an ID as an [authenticated user ID](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), but it does not fulfill hello user ID requirement for usage scenarios.</span></span>

## <a name="aspnet-apps-set-user-context-in-an-itelemetryinitializer"></a><span data-ttu-id="2bbd6-128">ASP.NET appar: Ange användarkontext i en ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="2bbd6-128">ASP.NET Apps: Set user context in an ITelemetryInitializer</span></span>

<span data-ttu-id="2bbd6-129">Skapa en telemetri initieraren som beskrivs i detalj [här](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer), och ange hello Context.User.Id och hello Context.Session.Id.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-129">Create a telemetry initializer, as described in detail [here](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer), and set hello Context.User.Id and hello Context.Session.Id.</span></span>

<span data-ttu-id="2bbd6-130">Det här exemplet anger hello användar-ID tooan ID som upphör att gälla efter hello-sessionen.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-130">This example sets hello user ID tooan identifier that expires after hello session.</span></span> <span data-ttu-id="2bbd6-131">Använd om möjligt ett användar-ID som kvarstår mellan sessioner.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-131">If possible, use a user ID that persists across sessions.</span></span>

<span data-ttu-id="2bbd6-132">*C#*</span><span class="sxs-lookup"><span data-stu-id="2bbd6-132">*C#*</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="2bbd6-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2bbd6-133">Next steps</span></span>
- <span data-ttu-id="2bbd6-134">tooenable användning inträffar, börja skicka [anpassade händelser](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) eller [sidvisningar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="2bbd6-134">tooenable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="2bbd6-135">Om du redan skickar anpassade händelser eller sidvyer utforska hello användning verktyg toolearn hur användare att använda tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2bbd6-135">If you already send custom events or page views, explore hello Usage tools toolearn how users use your service.</span></span>
    * [<span data-ttu-id="2bbd6-136">Översikt över användning</span><span class="sxs-lookup"><span data-stu-id="2bbd6-136">Usage overview</span></span>](app-insights-usage-overview.md)
    * [<span data-ttu-id="2bbd6-137">Användare, sessioner och händelser</span><span class="sxs-lookup"><span data-stu-id="2bbd6-137">Users, Sessions, and Events</span></span>](app-insights-usage-segmentation.md)
    * [<span data-ttu-id="2bbd6-138">Trattar</span><span class="sxs-lookup"><span data-stu-id="2bbd6-138">Funnels</span></span>](usage-funnels.md)
    * [<span data-ttu-id="2bbd6-139">Kvarhållning</span><span class="sxs-lookup"><span data-stu-id="2bbd6-139">Retention</span></span>](app-insights-usage-retention.md)
    * [<span data-ttu-id="2bbd6-140">Arbetsböcker</span><span class="sxs-lookup"><span data-stu-id="2bbd6-140">Workbooks</span></span>](app-insights-usage-workbooks.md)
