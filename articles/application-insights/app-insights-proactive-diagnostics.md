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
# <a name="smart-detection-in-application-insights"></a><span data-ttu-id="f89e2-103">Smart identifiering i Application Insights</span><span class="sxs-lookup"><span data-stu-id="f89e2-103">Smart Detection in Application Insights</span></span>
 <span data-ttu-id="f89e2-104">Smart identifiering varnar automatiskt dig för eventuella prestandaproblem i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="f89e2-104">Smart Detection automatically warns you of potential performance problems in your web application.</span></span> <span data-ttu-id="f89e2-105">Utför proaktiv analys av hello telemetri som appen skickar för[Programinsikter](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f89e2-105">It performs proactive analysis of hello telemetry that your app sends too[Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="f89e2-106">Om det finns en plötslig ökning av fel eller onormala mönster i klient- eller prestanda, får du en avisering.</span><span class="sxs-lookup"><span data-stu-id="f89e2-106">If there is a sudden rise in failure rates, or abnormal patterns in client or server performance, you get an alert.</span></span> <span data-ttu-id="f89e2-107">Den här funktionen krävs ingen konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f89e2-107">This feature needs no configuration.</span></span> <span data-ttu-id="f89e2-108">Den fungerar om programmet skickar tillräckligt med telemetri.</span><span class="sxs-lookup"><span data-stu-id="f89e2-108">It operates if your application sends enough telemetry.</span></span>

<span data-ttu-id="f89e2-109">Du kan komma åt varningar Smart av både från hello e-postmeddelanden visas, och hello Smart identifiering bladet.</span><span class="sxs-lookup"><span data-stu-id="f89e2-109">You can access Smart Detection alerts both from hello emails you receive, and from hello Smart Detection blade.</span></span>

## <a name="review-your-smart-detections"></a><span data-ttu-id="f89e2-110">Granska din Smart identifieringar</span><span class="sxs-lookup"><span data-stu-id="f89e2-110">Review your Smart Detections</span></span>
<span data-ttu-id="f89e2-111">Du kan identifiera identifieringar på två sätt:</span><span class="sxs-lookup"><span data-stu-id="f89e2-111">You can discover detections in two ways:</span></span>

* <span data-ttu-id="f89e2-112">**Du får ett e-postmeddelande** från Application Insights.</span><span class="sxs-lookup"><span data-stu-id="f89e2-112">**You receive an email** from Application Insights.</span></span> <span data-ttu-id="f89e2-113">Här är ett typexempel:</span><span class="sxs-lookup"><span data-stu-id="f89e2-113">Here's a typical example:</span></span>
  
    ![E-postavisering](./media/app-insights-proactive-diagnostics/03.png)
  
    <span data-ttu-id="f89e2-115">Klicka på hello stor knapp tooopen detalj i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="f89e2-115">Click hello big button tooopen more detail in hello portal.</span></span>
* <span data-ttu-id="f89e2-116">**hello Smart identifiering panelen** på din app översikt bladet visar antalet nya aviseringar.</span><span class="sxs-lookup"><span data-stu-id="f89e2-116">**hello Smart Detection tile** on your app's overview blade shows a count of recent alerts.</span></span> <span data-ttu-id="f89e2-117">Klicka på hello panelen toosee en lista över senaste aviseringarna.</span><span class="sxs-lookup"><span data-stu-id="f89e2-117">Click hello tile toosee a list of recent alerts.</span></span>

![Visa senaste identifieringar](./media/app-insights-proactive-diagnostics/04.png)

<span data-ttu-id="f89e2-119">Välj en avisering toosee information.</span><span class="sxs-lookup"><span data-stu-id="f89e2-119">Select an alert toosee its details.</span></span>

## <a name="what-problems-are-detected"></a><span data-ttu-id="f89e2-120">Vilka problem har identifierats?</span><span class="sxs-lookup"><span data-stu-id="f89e2-120">What problems are detected?</span></span>
<span data-ttu-id="f89e2-121">Det finns tre typer av identifiering:</span><span class="sxs-lookup"><span data-stu-id="f89e2-121">There are three kinds of detection:</span></span>

* <span data-ttu-id="f89e2-122">[Smartkort identifiering - fel avvikelser](app-insights-proactive-failure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="f89e2-122">[Smart detection - Failure Anomalies](app-insights-proactive-failure-diagnostics.md).</span></span> <span data-ttu-id="f89e2-123">Vi använder maskininlärning tooset hello förväntat antal misslyckade begäranden för din app korrelerar med belastning och andra faktorer.</span><span class="sxs-lookup"><span data-stu-id="f89e2-123">We use machine learning tooset hello expected rate of failed requests for your app, correlating with load and other factors.</span></span> <span data-ttu-id="f89e2-124">Om hello felintervall går utanför hello förväntade kuvert kan skicka vi en avisering.</span><span class="sxs-lookup"><span data-stu-id="f89e2-124">If hello failure rate goes outside hello expected envelope, we send an alert.</span></span>
* <span data-ttu-id="f89e2-125">[Smartkort identifiering - Prestandaavvikelser](app-insights-proactive-performance-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="f89e2-125">[Smart detection - Performance Anomalies](app-insights-proactive-performance-diagnostics.md).</span></span> <span data-ttu-id="f89e2-126">Du får meddelanden om svarstid för en åtgärd eller beroende varaktighet långsammare jämfört med toohistorical baslinjen eller om vi identifiera ett avvikande mönster i svarstid eller sidinläsningstiden.</span><span class="sxs-lookup"><span data-stu-id="f89e2-126">You get notifications if response time of an operation or dependency duration is slowing down compared toohistorical baseline or if we identify an anomalous pattern in response time or page load time.</span></span>   
* <span data-ttu-id="f89e2-127">[Smartkort identifiering - problem med Azure Cloud Service](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/).</span><span class="sxs-lookup"><span data-stu-id="f89e2-127">[Smart detection - Azure Cloud Service issues](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/).</span></span> <span data-ttu-id="f89e2-128">Du får aviseringar om din app finns i Azure Cloud Services och en rollinstans har startfel, ofta återvinning eller runtime kraschar.</span><span class="sxs-lookup"><span data-stu-id="f89e2-128">You get alerts if your app is hosted in Azure Cloud Services and a role instance has startup failures, frequent recycling, or runtime crashes.</span></span>

<span data-ttu-id="f89e2-129">(hello hjälplänkar i varje meddelande tar du toohello relevanta artiklar.)</span><span class="sxs-lookup"><span data-stu-id="f89e2-129">(hello help links in each notification take you toohello relevant articles.)</span></span>

## <a name="video"></a><span data-ttu-id="f89e2-130">Video</span><span class="sxs-lookup"><span data-stu-id="f89e2-130">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="f89e2-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f89e2-131">Next steps</span></span>
<span data-ttu-id="f89e2-132">Dessa verktyg för Nätverksdiagnostik hjälpa dig att inspektera hello telemetri från din app:</span><span class="sxs-lookup"><span data-stu-id="f89e2-132">These diagnostic tools help you inspect hello telemetry from your app:</span></span>

* [<span data-ttu-id="f89e2-133">Mått explorer</span><span class="sxs-lookup"><span data-stu-id="f89e2-133">Metric explorer</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="f89e2-134">Sök explorer</span><span class="sxs-lookup"><span data-stu-id="f89e2-134">Search explorer</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="f89e2-135">Analytics - frågespråket kraftfulla</span><span class="sxs-lookup"><span data-stu-id="f89e2-135">Analytics - powerful query language</span></span>](app-insights-analytics-tour.md)

<span data-ttu-id="f89e2-136">Smart identifiering är helt automatisk.</span><span class="sxs-lookup"><span data-stu-id="f89e2-136">Smart Detection is completely automatic.</span></span> <span data-ttu-id="f89e2-137">Men du kanske vill tooset in några fler aviseringar?</span><span class="sxs-lookup"><span data-stu-id="f89e2-137">But maybe you'd like tooset up some more alerts?</span></span>

* [<span data-ttu-id="f89e2-138">Manuellt konfigurerade mått aviseringar</span><span class="sxs-lookup"><span data-stu-id="f89e2-138">Manually configured metric alerts</span></span>](app-insights-alerts.md)
* [<span data-ttu-id="f89e2-139">Tillgänglighetstester för webbprogram</span><span class="sxs-lookup"><span data-stu-id="f89e2-139">Availability web tests</span></span>](app-insights-monitor-web-app-availability.md) 

