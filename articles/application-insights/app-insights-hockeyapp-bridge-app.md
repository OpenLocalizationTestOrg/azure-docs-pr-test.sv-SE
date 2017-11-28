---
title: aaaExploring HockeyApp data i Azure Application Insights | Microsoft Docs
description: "Analysera användnings- och prestanda för din Azure-app med Application Insights."
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: 97783cc6-67d6-465f-9926-cb9821f4176e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: bwren
ms.openlocfilehash: ed7cf99b48f5ec78d6b401bb954cfcd014b9d1f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a><span data-ttu-id="e8d13-103">Utforska HockeyApp data i Application Insights</span><span class="sxs-lookup"><span data-stu-id="e8d13-103">Exploring HockeyApp data in Application Insights</span></span>
<span data-ttu-id="e8d13-104">[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) rekommenderas hello plattform för övervakning av live stationära och mobila appar.</span><span class="sxs-lookup"><span data-stu-id="e8d13-104">[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) is hello recommended platform for monitoring live desktop and mobile apps.</span></span> <span data-ttu-id="e8d13-105">Från HockeyApp, kan du skicka anpassade och spåra telemetri toomonitor användning och hjälpa diagnos (i tillägg toogetting kraschdata).</span><span class="sxs-lookup"><span data-stu-id="e8d13-105">From HockeyApp, you can send custom and trace telemetry toomonitor usage and assist in diagnosis (in addition toogetting crash data).</span></span> <span data-ttu-id="e8d13-106">Dataströmmen telemetri kan efterfrågas med hello kraftfulla [Analytics](app-insights-analytics.md) funktion i [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e8d13-106">This stream of telemetry can be queried using hello powerful [Analytics](app-insights-analytics.md) feature of [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="e8d13-107">Dessutom kan du [exportera hello anpassade och spårningstelemetri](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="e8d13-107">In addition, you can [export hello custom and trace telemetry](app-insights-export-telemetry.md).</span></span> <span data-ttu-id="e8d13-108">tooenable dessa funktioner, som du ställa in en brygga som vidarebefordrar HockeyApp anpassade data tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="e8d13-108">tooenable these features, you set up a bridge that relays HockeyApp custom data tooApplication Insights.</span></span>

## <a name="hello-hockeyapp-bridge-app"></a><span data-ttu-id="e8d13-109">Hej HockeyApp Bridge app</span><span class="sxs-lookup"><span data-stu-id="e8d13-109">hello HockeyApp Bridge app</span></span>
<span data-ttu-id="e8d13-110">Hej HockeyApp Bridge App är hello core funktion som gör att du tooaccess dina anpassade HockeyApp och spåra telemetri i Application Insights via hello Analytics och löpande Export funktioner.</span><span class="sxs-lookup"><span data-stu-id="e8d13-110">hello HockeyApp Bridge App is hello core feature that enables you tooaccess your HockeyApp custom and trace telemetry in Application Insights through hello Analytics and Continuous Export features.</span></span> <span data-ttu-id="e8d13-111">Anpassad och spåra händelser som samlas in av HockeyApp efter hello generering av hello HockeyApp Bridge App ska vara tillgänglig från dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="e8d13-111">Custom and trace events collected by HockeyApp after hello creation of hello HockeyApp Bridge App will be accessible from these features.</span></span> <span data-ttu-id="e8d13-112">Nu ska vi se hur tooset en av apparna Bridge.</span><span class="sxs-lookup"><span data-stu-id="e8d13-112">Let’s see how tooset up one of these Bridge Apps.</span></span>

<span data-ttu-id="e8d13-113">Öppna kontoinställningar, i HockeyApp, [API token](https://rink.hockeyapp.net/manage/auth_tokens).</span><span class="sxs-lookup"><span data-stu-id="e8d13-113">In HockeyApp, open Account Settings, [API Tokens](https://rink.hockeyapp.net/manage/auth_tokens).</span></span> <span data-ttu-id="e8d13-114">Skapa en ny token eller återanvända en befintlig.</span><span class="sxs-lookup"><span data-stu-id="e8d13-114">Either create a new token or reuse an existing one.</span></span> <span data-ttu-id="e8d13-115">hello lägsta behörighet som krävs är ”skrivskyddade”.</span><span class="sxs-lookup"><span data-stu-id="e8d13-115">hello minimum rights required are "read only".</span></span> <span data-ttu-id="e8d13-116">Ta en kopia av hello API-token.</span><span class="sxs-lookup"><span data-stu-id="e8d13-116">Take a copy of hello API token.</span></span>

![Hämta ett HockeyApp API-token](./media/app-insights-hockeyapp-bridge-app/01.png)

<span data-ttu-id="e8d13-118">Öppna hello Microsoft Azure-portalen och [skapa Application Insights-resurs](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="e8d13-118">Open hello Microsoft Azure portal and [create an Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="e8d13-119">Ange program för ”HockeyApp bridge program”:</span><span class="sxs-lookup"><span data-stu-id="e8d13-119">Set Application Type too“HockeyApp bridge application”:</span></span>

![Ny Application Insights-resurs](./media/app-insights-hockeyapp-bridge-app/02.png)

<span data-ttu-id="e8d13-121">Du behöver inte tooset ett namn - detta in automatiskt från hello HockeyApp namn.</span><span class="sxs-lookup"><span data-stu-id="e8d13-121">You don't need tooset a name - this will automatically be set from hello HockeyApp name.</span></span>

<span data-ttu-id="e8d13-122">Hej HockeyApp bridge fält visas.</span><span class="sxs-lookup"><span data-stu-id="e8d13-122">hello HockeyApp bridge fields appear.</span></span> 

![Ange bridge fält](./media/app-insights-hockeyapp-bridge-app/03.png)

<span data-ttu-id="e8d13-124">Ange hello HockeyApp token som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="e8d13-124">Enter hello HockeyApp token you noted earlier.</span></span> <span data-ttu-id="e8d13-125">Den här åtgärden fyller hello ”HockeyApp-program” nedrullningsbar meny med alla HockeyApp-program.</span><span class="sxs-lookup"><span data-stu-id="e8d13-125">This action populates hello “HockeyApp Application” dropdown menu with all your HockeyApp applications.</span></span> <span data-ttu-id="e8d13-126">Välj hello en du vill toouse och fullständig hello resten av hello fält.</span><span class="sxs-lookup"><span data-stu-id="e8d13-126">Select hello one you want toouse, and complete hello remainder of hello fields.</span></span> 

<span data-ttu-id="e8d13-127">Öppna hello ny resurs.</span><span class="sxs-lookup"><span data-stu-id="e8d13-127">Open hello new resource.</span></span> 

<span data-ttu-id="e8d13-128">Observera att det tar en stund hello data flödar toostart.</span><span class="sxs-lookup"><span data-stu-id="e8d13-128">Note that hello data takes a while toostart flowing.</span></span>

![Application Insights-resurs som väntar på data](./media/app-insights-hockeyapp-bridge-app/04.png)

<span data-ttu-id="e8d13-130">Klart!</span><span class="sxs-lookup"><span data-stu-id="e8d13-130">That’s it!</span></span> <span data-ttu-id="e8d13-131">Anpassade och spåra data som samlas in i appen HockeyApp instrumenterats från och med nu är nu också tillgängliga tooyou i hello Analytics och löpande Export funktioner i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="e8d13-131">Custom and trace data collected in your HockeyApp-instrumented app from this point forward is now also available tooyou in hello Analytics and Continuous Export features of Application Insights.</span></span>

<span data-ttu-id="e8d13-132">Nu ska vi gå igenom var och en av dessa funktioner finns nu tillgänglig tooyou.</span><span class="sxs-lookup"><span data-stu-id="e8d13-132">Let’s briefly review each of these features now available tooyou.</span></span>

## <a name="analytics"></a><span data-ttu-id="e8d13-133">Analys</span><span class="sxs-lookup"><span data-stu-id="e8d13-133">Analytics</span></span>
<span data-ttu-id="e8d13-134">Analytics är ett kraftfullt verktyg för ad hoc-frågor till dina data, så att du toodiagnose och analysera dina telemetri och identifiera snabbt rotorsaker och mönster.</span><span class="sxs-lookup"><span data-stu-id="e8d13-134">Analytics is a powerful tool for ad-hoc querying of your data, allowing you toodiagnose and analyze your telemetry and quickly discover root causes and patterns.</span></span>

![Analys](./media/app-insights-hockeyapp-bridge-app/05.png)

* [<span data-ttu-id="e8d13-136">Mer information om Analytics</span><span class="sxs-lookup"><span data-stu-id="e8d13-136">Learn more about Analytics</span></span>](app-insights-analytics-tour.md)

## <a name="continuous-export"></a><span data-ttu-id="e8d13-137">Löpande export</span><span class="sxs-lookup"><span data-stu-id="e8d13-137">Continuous export</span></span>
<span data-ttu-id="e8d13-138">Löpande Export kan du tooexport dina data till en Azure Blob Storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="e8d13-138">Continuous Export allows you tooexport your data into an Azure Blob Storage container.</span></span> <span data-ttu-id="e8d13-139">Detta är användbart om du behöver tookeep informationen under längre tid än hello kvarhållningsperiod för närvarande erbjuds av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="e8d13-139">This is very useful if you need tookeep your data for longer than hello retention period currently offered by Application Insights.</span></span> <span data-ttu-id="e8d13-140">Du kan hålla hello data i blob storage kan bearbeta till en SQL-databas eller din önskade datalagerlösning.</span><span class="sxs-lookup"><span data-stu-id="e8d13-140">You can keep hello data in blob storage, process it into a SQL Database, or your preferred data warehousing solution.</span></span>

[<span data-ttu-id="e8d13-141">Mer information om löpande Export</span><span class="sxs-lookup"><span data-stu-id="e8d13-141">Learn more about Continuous Export</span></span>](app-insights-export-telemetry.md)

## <a name="next-steps"></a><span data-ttu-id="e8d13-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e8d13-142">Next steps</span></span>
* [<span data-ttu-id="e8d13-143">Tillämpa tooyour analysdata</span><span class="sxs-lookup"><span data-stu-id="e8d13-143">Apply Analytics tooyour data</span></span>](app-insights-analytics-tour.md)

