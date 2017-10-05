---
title: Utforska HockeyApp data i Azure Application Insights | Microsoft Docs
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
ms.openlocfilehash: 450ca10613d137393090578619f3766734d1d493
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a><span data-ttu-id="13d99-103">Utforska HockeyApp data i Application Insights</span><span class="sxs-lookup"><span data-stu-id="13d99-103">Exploring HockeyApp data in Application Insights</span></span>
<span data-ttu-id="13d99-104">[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) är den rekommenderade plattformen för övervakning av live stationära och mobila appar.</span><span class="sxs-lookup"><span data-stu-id="13d99-104">[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) is the recommended platform for monitoring live desktop and mobile apps.</span></span> <span data-ttu-id="13d99-105">Du kan skicka anpassade från HockeyApp och spåra telemetri för att övervaka användningen och hjälpa diagnostisera (förutom komma kraschdata).</span><span class="sxs-lookup"><span data-stu-id="13d99-105">From HockeyApp, you can send custom and trace telemetry to monitor usage and assist in diagnosis (in addition to getting crash data).</span></span> <span data-ttu-id="13d99-106">Dataströmmen telemetri kan efterfrågas med hjälp av den kraftfulla [Analytics](app-insights-analytics.md) funktion i [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="13d99-106">This stream of telemetry can be queried using the powerful [Analytics](app-insights-analytics.md) feature of [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="13d99-107">Dessutom kan du [exportera ett anpassat och spåra telemetri](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="13d99-107">In addition, you can [export the custom and trace telemetry](app-insights-export-telemetry.md).</span></span> <span data-ttu-id="13d99-108">Om du vill aktivera de här funktionerna kan du ställa in en brygga som vidarebefordrar HockeyApp anpassade data till Application Insights.</span><span class="sxs-lookup"><span data-stu-id="13d99-108">To enable these features, you set up a bridge that relays HockeyApp custom data to Application Insights.</span></span>

## <a name="the-hockeyapp-bridge-app"></a><span data-ttu-id="13d99-109">HockeyApp Bridge appen</span><span class="sxs-lookup"><span data-stu-id="13d99-109">The HockeyApp Bridge app</span></span>
<span data-ttu-id="13d99-110">HockeyApp Bridge appen är core-funktion som gör att du kan komma åt dina anpassade HockeyApp och spåra telemetri i Application Insights via analyser och löpande Export funktioner.</span><span class="sxs-lookup"><span data-stu-id="13d99-110">The HockeyApp Bridge App is the core feature that enables you to access your HockeyApp custom and trace telemetry in Application Insights through the Analytics and Continuous Export features.</span></span> <span data-ttu-id="13d99-111">Anpassad och spåra händelser som samlas in av HockeyApp efter skapandet av HockeyApp Bridge appen är tillgänglig från dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="13d99-111">Custom and trace events collected by HockeyApp after the creation of the HockeyApp Bridge App will be accessible from these features.</span></span> <span data-ttu-id="13d99-112">Låt oss se hur du ställer in en av apparna Bridge.</span><span class="sxs-lookup"><span data-stu-id="13d99-112">Let’s see how to set up one of these Bridge Apps.</span></span>

<span data-ttu-id="13d99-113">Öppna kontoinställningar, i HockeyApp, [API token](https://rink.hockeyapp.net/manage/auth_tokens).</span><span class="sxs-lookup"><span data-stu-id="13d99-113">In HockeyApp, open Account Settings, [API Tokens](https://rink.hockeyapp.net/manage/auth_tokens).</span></span> <span data-ttu-id="13d99-114">Skapa en ny token eller återanvända en befintlig.</span><span class="sxs-lookup"><span data-stu-id="13d99-114">Either create a new token or reuse an existing one.</span></span> <span data-ttu-id="13d99-115">Den minimala behörigheten som krävs är ”skrivskyddade”.</span><span class="sxs-lookup"><span data-stu-id="13d99-115">The minimum rights required are "read only".</span></span> <span data-ttu-id="13d99-116">Ta en kopia av API-token.</span><span class="sxs-lookup"><span data-stu-id="13d99-116">Take a copy of the API token.</span></span>

![Hämta ett HockeyApp API-token](./media/app-insights-hockeyapp-bridge-app/01.png)

<span data-ttu-id="13d99-118">Öppna Microsoft Azure-portalen och [skapa Application Insights-resurs](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="13d99-118">Open the Microsoft Azure portal and [create an Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="13d99-119">Ange typ av program ”HockeyApp bridge program”:</span><span class="sxs-lookup"><span data-stu-id="13d99-119">Set Application Type to “HockeyApp bridge application”:</span></span>

![Ny Application Insights-resurs](./media/app-insights-hockeyapp-bridge-app/02.png)

<span data-ttu-id="13d99-121">Du behöver inte ange ett namn - detta in automatiskt från HockeyApp namn.</span><span class="sxs-lookup"><span data-stu-id="13d99-121">You don't need to set a name - this will automatically be set from the HockeyApp name.</span></span>

<span data-ttu-id="13d99-122">Fälten HockeyApp bridge visas.</span><span class="sxs-lookup"><span data-stu-id="13d99-122">The HockeyApp bridge fields appear.</span></span> 

![Ange bridge fält](./media/app-insights-hockeyapp-bridge-app/03.png)

<span data-ttu-id="13d99-124">Ange HockeyApp-token som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="13d99-124">Enter the HockeyApp token you noted earlier.</span></span> <span data-ttu-id="13d99-125">Den här åtgärden fyller listrutan ”HockeyApp-program” med alla HockeyApp-program.</span><span class="sxs-lookup"><span data-stu-id="13d99-125">This action populates the “HockeyApp Application” dropdown menu with all your HockeyApp applications.</span></span> <span data-ttu-id="13d99-126">Väljer du det alternativ som du vill använda och Slutför resten av fälten.</span><span class="sxs-lookup"><span data-stu-id="13d99-126">Select the one you want to use, and complete the remainder of the fields.</span></span> 

<span data-ttu-id="13d99-127">Öppna den nya resursen.</span><span class="sxs-lookup"><span data-stu-id="13d99-127">Open the new resource.</span></span> 

<span data-ttu-id="13d99-128">Observera att data tar en stund att börjar flöda.</span><span class="sxs-lookup"><span data-stu-id="13d99-128">Note that the data takes a while to start flowing.</span></span>

![Application Insights-resurs som väntar på data](./media/app-insights-hockeyapp-bridge-app/04.png)

<span data-ttu-id="13d99-130">Klart!</span><span class="sxs-lookup"><span data-stu-id="13d99-130">That’s it!</span></span> <span data-ttu-id="13d99-131">Anpassade och spåra data som samlas in i appen HockeyApp instrumenterats från och med nu är nu också tillgängliga i funktionerna för analys och löpande Export av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="13d99-131">Custom and trace data collected in your HockeyApp-instrumented app from this point forward is now also available to you in the Analytics and Continuous Export features of Application Insights.</span></span>

<span data-ttu-id="13d99-132">Nu ska vi gå igenom de olika funktionerna är nu tillgängliga för dig.</span><span class="sxs-lookup"><span data-stu-id="13d99-132">Let’s briefly review each of these features now available to you.</span></span>

## <a name="analytics"></a><span data-ttu-id="13d99-133">Analys</span><span class="sxs-lookup"><span data-stu-id="13d99-133">Analytics</span></span>
<span data-ttu-id="13d99-134">Analytics är ett kraftfullt verktyg för ad hoc-frågor till dina data så att du kan felsöka och analysera dina telemetri och snabbt identifiera rotorsaker och mönster.</span><span class="sxs-lookup"><span data-stu-id="13d99-134">Analytics is a powerful tool for ad-hoc querying of your data, allowing you to diagnose and analyze your telemetry and quickly discover root causes and patterns.</span></span>

![Analys](./media/app-insights-hockeyapp-bridge-app/05.png)

* [<span data-ttu-id="13d99-136">Mer information om Analytics</span><span class="sxs-lookup"><span data-stu-id="13d99-136">Learn more about Analytics</span></span>](app-insights-analytics-tour.md)

## <a name="continuous-export"></a><span data-ttu-id="13d99-137">Löpande export</span><span class="sxs-lookup"><span data-stu-id="13d99-137">Continuous export</span></span>
<span data-ttu-id="13d99-138">Löpande Export kan du exportera dina data till en Azure Blob Storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="13d99-138">Continuous Export allows you to export your data into an Azure Blob Storage container.</span></span> <span data-ttu-id="13d99-139">Detta är användbart om du vill bevara data längre än kvarhållningsperioden för närvarande erbjuds av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="13d99-139">This is very useful if you need to keep your data for longer than the retention period currently offered by Application Insights.</span></span> <span data-ttu-id="13d99-140">Du kan behålla data i blob storage kan bearbeta till en SQL-databas eller din önskade datalagerlösning.</span><span class="sxs-lookup"><span data-stu-id="13d99-140">You can keep the data in blob storage, process it into a SQL Database, or your preferred data warehousing solution.</span></span>

[<span data-ttu-id="13d99-141">Mer information om löpande Export</span><span class="sxs-lookup"><span data-stu-id="13d99-141">Learn more about Continuous Export</span></span>](app-insights-export-telemetry.md)

## <a name="next-steps"></a><span data-ttu-id="13d99-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="13d99-142">Next steps</span></span>
* [<span data-ttu-id="13d99-143">Tillämpa Analytics till dina data</span><span class="sxs-lookup"><span data-stu-id="13d99-143">Apply Analytics to your data</span></span>](app-insights-analytics-tour.md)

