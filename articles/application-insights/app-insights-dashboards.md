---
title: Instrumentpaneler och navigering i Azure Application Insights | Microsoft Docs
description: "Skapa vyer av din nyckel APM diagram och frågor."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 39b0701b-2fec-4683-842a-8a19424f67bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 9987f04e7e71df5fe10c8bc209a390cb940ec4f2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="navigation-and-dashboards-in-the-application-insights-portal"></a><span data-ttu-id="3f0b7-103">Navigering och instrumentpaneler i Application Insights-portalen</span><span class="sxs-lookup"><span data-stu-id="3f0b7-103">Navigation and Dashboards in the Application Insights portal</span></span>
<span data-ttu-id="3f0b7-104">När du har [konfigurera Application Insights i ditt projekt](app-insights-overview.md), telemetridata om prestanda och användning av din app visas i ditt projekt Application Insights-resurs i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3f0b7-104">After you have [set up Application Insights on your project](app-insights-overview.md), telemetry data about your app's performance and usage will appear in your project's Application Insights resource in the [Azure portal](https://portal.azure.com).</span></span>

## <a name="find-your-telemetry"></a><span data-ttu-id="3f0b7-105">Hitta din telemetri</span><span class="sxs-lookup"><span data-stu-id="3f0b7-105">Find your telemetry</span></span>
<span data-ttu-id="3f0b7-106">Logga in på den [Azure-portalen](https://portal.azure.com) och navigera till Application Insights-resursen som du skapade för din app.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-106">Sign in to the [Azure portal](https://portal.azure.com) and navigate to the Application Insights resource that you created for your app.</span></span>

![Klicka på Bläddra, Välj Application Insights och sedan din app.](./media/app-insights-dashboards/00-start.png)

<span data-ttu-id="3f0b7-108">Översikt över bladet för din app (sidan) visas en sammanfattning av diagnostiska nyckelvärden för din app och är en gateway för andra funktioner i portalen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-108">The overview blade (page) for your app shows a summary of the key diagnostic metrics of your app, and is a gateway to the other features of the portal.</span></span>

![Större vägar för att visa din telemetri](./media/app-insights-dashboards/010-oview.png)

<span data-ttu-id="3f0b7-110">Du kan anpassa någon av de diagram och rutnät och fästa dem på en instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-110">You can customize any of the charts and grids and pin them to a dashboard.</span></span> <span data-ttu-id="3f0b7-111">På så sätt kan du kan hämta nyckeln telemetri från olika appar på en central instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-111">That way, you can bring together the key telemetry from different apps on a central dashboard.</span></span>

## <a name="dashboards"></a><span data-ttu-id="3f0b7-112">Instrumentpaneler</span><span class="sxs-lookup"><span data-stu-id="3f0b7-112">Dashboards</span></span>
<span data-ttu-id="3f0b7-113">Första du se när du loggar in på den [Microsoft Azure-portalen](https://portal.azure.com) är en instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-113">The first thing you see after you sign in to the [Microsoft Azure portal](https://portal.azure.com) is a dashboard.</span></span> <span data-ttu-id="3f0b7-114">Här kan du sammanfoga diagram som är viktigast för dig i alla dina Azure-resurser, inklusive telemetri från [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3f0b7-114">Here you can bring together the charts that are most important to you across all your Azure resources, including telemetry from [Azure Application Insights](app-insights-overview.md).</span></span>

![En anpassad instrumentpanel.](./media/app-insights-dashboards/31.png)

1. <span data-ttu-id="3f0b7-116">**Navigera till specifika resurser** som din app i Application Insights: använda fältet till vänster.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-116">**Navigate to specific resources** such as your app in Application Insights: Use the left bar.</span></span>
2. <span data-ttu-id="3f0b7-117">**Tillbaka till den aktuella instrumentpanelen**, eller växla till andra vyer för senaste: Använd den nedrullningsbara menyn på upp till vänster.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-117">**Return to the current dashboard**, or switch to other recent views: Use the drop-down menu at top left.</span></span>
3. <span data-ttu-id="3f0b7-118">**Växla instrumentpaneler**: Använd den nedrullningsbara menyn på instrumentpanelen rubriken</span><span class="sxs-lookup"><span data-stu-id="3f0b7-118">**Switch dashboards**: Use the drop-down menu on the dashboard title</span></span>
4. <span data-ttu-id="3f0b7-119">**Skapa, redigera och dela instrumentpaneler** i verktygsfältet instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-119">**Create, edit, and share dashboards** in the dashboard toolbar.</span></span>
5. <span data-ttu-id="3f0b7-120">**Redigera instrumentpanelen**: hovra över en panel och använda dess översta raden för att flytta, anpassa eller ta bort den.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-120">**Edit the dashboard**: Hover over a tile and then use its top bar to move, customize, or remove it.</span></span>

## <a name="add-to-a-dashboard"></a><span data-ttu-id="3f0b7-121">Lägg till en instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="3f0b7-121">Add to a dashboard</span></span>
<span data-ttu-id="3f0b7-122">När du tittar på ett blad eller uppsättning diagram som är särskilt intressanta, kan du fästa en kopia av den på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-122">When you're looking at a blade or set of charts that's particularly interesting, you can pin a copy of it to the dashboard.</span></span> <span data-ttu-id="3f0b7-123">Ser du den nästa gång du kommer tillbaka det.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-123">You'll see it next time you return there.</span></span>

![Hovra över den och klicka på ”...” i rubriken för att fästa ett diagram.](./media/app-insights-dashboards/33.png)

1. <span data-ttu-id="3f0b7-125">PIN-kod diagram på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-125">Pin chart to dashboard.</span></span> <span data-ttu-id="3f0b7-126">En kopia av diagrammet visas på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-126">A copy of the chart appears on the dashboard.</span></span>
2. <span data-ttu-id="3f0b7-127">Fäst hela bladet på instrumentpanelen, visas den på instrumentpanelen som en panel som du kan klicka på via.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-127">Pin the whole blade to the dashboard - it appears on the dashboard as a tile that you can click through.</span></span>
3. <span data-ttu-id="3f0b7-128">Klicka på det övre vänstra hörnet för att återgå till den aktuella instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-128">Click the top left corner to return to the current dashboard.</span></span> <span data-ttu-id="3f0b7-129">Du kan sedan använda den nedrullningsbara menyn för att återgå till den aktuella vyn.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-129">Then you can use the drop-down menu to return to the current view.</span></span>

<span data-ttu-id="3f0b7-130">Observera att diagram grupperas i panelerna: en panel kan innehålla fler än ett schema.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-130">Notice that charts are grouped into tiles: a tile can contain more than one chart.</span></span> <span data-ttu-id="3f0b7-131">Du kan fästa panelen hela på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-131">You pin the whole tile to the dashboard.</span></span>

<span data-ttu-id="3f0b7-132">Diagrammet uppdateras automatiskt med en frekvens som är beroende av diagrammets tidsintervall:</span><span class="sxs-lookup"><span data-stu-id="3f0b7-132">The chart is automatically refreshed with a frequency that depends on the chart's time range:</span></span>

* <span data-ttu-id="3f0b7-133">Tid vara upp till 1 timme: uppdatera var femte minut</span><span class="sxs-lookup"><span data-stu-id="3f0b7-133">Time range up to 1 hour: Refresh every 5 minutes</span></span>
* <span data-ttu-id="3f0b7-134">Tidsintervallet 1-24 timmar: uppdatera var 15: e minut</span><span class="sxs-lookup"><span data-stu-id="3f0b7-134">Time range 1 - 24 hours: Refresh every 15 minutes</span></span>
* <span data-ttu-id="3f0b7-135">Tidsintervallet ovan 24 timmar: (tidsintervallet) / 60.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-135">Time range above 24 hours: (Time range)/60.</span></span>

### <a name="pin-any-query-in-analytics"></a><span data-ttu-id="3f0b7-136">Fästa en fråga i Analytics</span><span class="sxs-lookup"><span data-stu-id="3f0b7-136">Pin any query in Analytics</span></span>
<span data-ttu-id="3f0b7-137">Du kan också [fästa Analytics](app-insights-analytics-using.md#pin-to-dashboard) diagram till en [delade](#share-dashboards-with-your-team) instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-137">You can also [pin Analytics](app-insights-analytics-using.md#pin-to-dashboard) charts to a [shared](#share-dashboards-with-your-team) dashboard.</span></span> <span data-ttu-id="3f0b7-138">På så sätt kan du lägga till diagram av en skadlig fråga tillsammans med mätvärdena som är standard.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-138">This allows you to add charts of any arbitrary query alongside the standard metrics.</span></span> 

<span data-ttu-id="3f0b7-139">Resultaten beräknas automatiskt varje timme.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-139">Results are automatically recalculated every hour.</span></span> <span data-ttu-id="3f0b7-140">Klicka på ikonen Uppdatera i diagrammet för att beräkna om omedelbart.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-140">Click the Refresh icon on the chart to recalculate immediately.</span></span> <span data-ttu-id="3f0b7-141">(Uppdatera webbläsaren omberäknar inte.)</span><span class="sxs-lookup"><span data-stu-id="3f0b7-141">(Browser refresh doesn't recalculate.)</span></span>

## <a name="adjust-a-tile-on-the-dashboard"></a><span data-ttu-id="3f0b7-142">Justera en panel på instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="3f0b7-142">Adjust a tile on the dashboard</span></span>
<span data-ttu-id="3f0b7-143">Du kan justera när en panel på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-143">Once a tile is on the dashboard, you can adjust it.</span></span>

![Hovra över ett diagram om du vill redigera.](./media/app-insights-dashboards/36.png)

1. <span data-ttu-id="3f0b7-145">Lägga till ett diagram i panelen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-145">Add a chart to the tile.</span></span>
2. <span data-ttu-id="3f0b7-146">Ange mått, gruppera efter dimension och format (tabell, diagram) i ett diagram.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-146">Set the metric, group-by dimension and style (table, graph) of a chart.</span></span>
3. <span data-ttu-id="3f0b7-147">Dra över diagrammet för att zooma in den. Klicka på knappen Ångra om du vill återställa timespan; Ange filteregenskaper för diagram på panelen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-147">Drag across the diagram to zoom in; click the undo button to reset the timespan; set filter properties for the charts on the tile.</span></span>
4. <span data-ttu-id="3f0b7-148">Ange panelrubriken.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-148">Set tile title.</span></span>

<span data-ttu-id="3f0b7-149">Paneler fästs från mått explorer blad har fler redigera alternativ än paneler fästs från en översikt över bladet.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-149">Tiles pinned from metric explorer blades have more editing options than tiles pinned from an Overview blade.</span></span>

<span data-ttu-id="3f0b7-150">Den ursprungliga panelen som du har fäst påverkas inte av ändringarna.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-150">The original tile that you pinned isn't affected by your edits.</span></span>

## <a name="switch-between-dashboards"></a><span data-ttu-id="3f0b7-151">Växla mellan instrumentpaneler</span><span class="sxs-lookup"><span data-stu-id="3f0b7-151">Switch between dashboards</span></span>
<span data-ttu-id="3f0b7-152">Du kan spara mer än en instrumentpanel och växla mellan dem.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-152">You can save more than one dashboard and switch between them.</span></span> <span data-ttu-id="3f0b7-153">När du fäster ett diagram eller bladet, läggs de till den aktuella instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-153">When you pin a chart or blade, they're added to the current dashboard.</span></span>

![Växla mellan instrumentpaneler, klicka på instrumentpanelen och välj en sparad instrumentpanel.](./media/app-insights-dashboards/32.png)

<span data-ttu-id="3f0b7-157">Du kan till exempel ha en instrumentpanel om du vill visa hela skärmen i rummet team och en annan för allmänna utvecklingen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-157">For example, you might have one dashboard for displaying full screen in the team room, and another for general development.</span></span>

<span data-ttu-id="3f0b7-158">Ett blad visas som en panel på instrumentpanelen: genom att klicka på Gå till bladet.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-158">On the dashboard, a blade appears as a tile: click it to go to the blade.</span></span> <span data-ttu-id="3f0b7-159">Ett diagram replikerar diagram på den ursprungliga platsen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-159">A chart replicates the chart in its original location.</span></span>

![Klicka på en panel för att öppna bladet representerar](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a><span data-ttu-id="3f0b7-161">Dela instrumentpaneler</span><span class="sxs-lookup"><span data-stu-id="3f0b7-161">Share dashboards</span></span>
<span data-ttu-id="3f0b7-162">När du har skapat en instrumentpanel kan dela du den med andra användare.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-162">When you've created a dashboard, you can share it with other users.</span></span>

![Klicka på resurs i rubriken instrumentpanelen](./media/app-insights-dashboards/41.png)

<span data-ttu-id="3f0b7-164">Lär dig mer om [roller och åtkomstkontroll](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="3f0b7-164">Learn about [Roles and access control](app-insights-resources-roles-access-control.md).</span></span>

## <a name="app-navigation"></a><span data-ttu-id="3f0b7-165">App-navigering</span><span class="sxs-lookup"><span data-stu-id="3f0b7-165">App navigation</span></span>
<span data-ttu-id="3f0b7-166">Översikt över bladet är gateway till mer information om din app.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-166">The overview blade is the gateway to more information about your app.</span></span>

* <span data-ttu-id="3f0b7-167">**Alla diagram eller panelen** – Klicka på panelen eller diagram om du vill se mer information om vad som visas.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-167">**Any chart or tile** - Click any tile or chart to see more detail about what it displays.</span></span>

### <a name="overview-blade-buttons"></a><span data-ttu-id="3f0b7-168">Översikt över bladet knappar</span><span class="sxs-lookup"><span data-stu-id="3f0b7-168">Overview blade buttons</span></span>
![Översikt över bladet övre navigeringsfält](./media/app-insights-dashboards/app-overview-top-nav.png)

* <span data-ttu-id="3f0b7-170">[**Metrics Explorer** ](app-insights-metrics-explorer.md) -skapa egna diagram av prestanda och användning.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-170">[**Metrics Explorer**](app-insights-metrics-explorer.md) - Create your own charts of performance and usage.</span></span>
* <span data-ttu-id="3f0b7-171">[**Sök** ](app-insights-diagnostic-search.md) – undersöka specifika instanser av händelser, till exempel begäranden, undantag, eller logga in spårningar.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-171">[**Search**](app-insights-diagnostic-search.md) - Investigate specific instances of events such as requests, exceptions, or log traces.</span></span>
* <span data-ttu-id="3f0b7-172">[**Analytics** ](app-insights-analytics.md) -kraftfulla frågor via din telemetri.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-172">[**Analytics**](app-insights-analytics.md) - Powerful queries over your telemetry.</span></span>
* <span data-ttu-id="3f0b7-173">**Tidsintervallet** -Justera intervall som visas av alla diagram på bladet.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-173">**Time range** - Adjust the range displayed by all the charts on the blade.</span></span>
* <span data-ttu-id="3f0b7-174">**Ta bort** -ta bort Application Insights-resurs för den här appen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-174">**Delete** - Delete the Application Insights resource for this app.</span></span> <span data-ttu-id="3f0b7-175">Du bör också ta bort Application Insights-paket från din Appkod, eller redigera den [instrumentation nyckeln](app-insights-create-new-resource.md#copy-the-instrumentation-key) i din app att dirigera telemetri till en annan Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-175">You should also either remove the Application Insights packages from your app code, or edit the [instrumentation key](app-insights-create-new-resource.md#copy-the-instrumentation-key) in your app to direct telemetry to a different Application Insights resource.</span></span>

### <a name="essentials-tab"></a><span data-ttu-id="3f0b7-176">Fliken Essentials</span><span class="sxs-lookup"><span data-stu-id="3f0b7-176">Essentials tab</span></span>
* <span data-ttu-id="3f0b7-177">[Instrumentation nyckeln](app-insights-create-new-resource.md#copy-the-instrumentation-key) -identifierar den här appen resurs.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-177">[Instrumentation key](app-insights-create-new-resource.md#copy-the-instrumentation-key) - Identifies this app resource.</span></span>
* <span data-ttu-id="3f0b7-178">Priser för - och göra funktioner tillgängliga och ange volym versaler.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-178">Pricing - Make features available and set volume caps.</span></span>

### <a name="app-navigation-bar"></a><span data-ttu-id="3f0b7-179">Navigeringsfältet i appen</span><span class="sxs-lookup"><span data-stu-id="3f0b7-179">App navigation bar</span></span>
![Vänstra navigeringsfältet](./media/app-insights-dashboards/app-left-nav-bar.png)

* <span data-ttu-id="3f0b7-181">**Översikt över** -gå tillbaka till bladet Översikt.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-181">**Overview** - Return to the app overview blade.</span></span>
* <span data-ttu-id="3f0b7-182">**Aktivitetsloggen** -aviseringar och Azure administrativa händelser.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-182">**Activity log** - Alerts and Azure administrative events.</span></span>
* <span data-ttu-id="3f0b7-183">[**Åtkomstkontroll** ](app-insights-resources-roles-access-control.md) -ge åtkomst till teammedlemmar och andra.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-183">[**Access control**](app-insights-resources-roles-access-control.md) - Provide access to team members and others.</span></span>
* <span data-ttu-id="3f0b7-184">[**Taggar** ](../azure-resource-manager/resource-group-using-tags.md) -använda taggar för att gruppera din app med andra.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-184">[**Tags**](../azure-resource-manager/resource-group-using-tags.md) - Use tags to group your app with others.</span></span>

<span data-ttu-id="3f0b7-185">UNDERSÖK</span><span class="sxs-lookup"><span data-stu-id="3f0b7-185">INVESTIGATE</span></span>

* <span data-ttu-id="3f0b7-186">[**Programavbildningen** ](app-insights-app-map.md) -aktiv karta med komponenter för programmet, som härletts från beroendeinformationen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-186">[**Application map**](app-insights-app-map.md) - Active map showing the components of your application, derived from the dependency information.</span></span>
* <span data-ttu-id="3f0b7-187">[**Identifiering för smartkort** ](app-insights-proactive-diagnostics.md) -granska de senaste prestanda aviseringarna.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-187">[**Smart Detection**](app-insights-proactive-diagnostics.md) - Review recent performance alerts.</span></span>
* <span data-ttu-id="3f0b7-188">[**Direktsänd dataström** ](app-insights-live-stream.md) – en fast uppsättning nästan omedelbar statistik, användbart när du distribuerar en ny version eller felsökning.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-188">[**Live Stream**](app-insights-live-stream.md) - A fixed set of near-instant metrics, useful when deploying a new build or debugging.</span></span>
* <span data-ttu-id="3f0b7-189">[**Tillgänglighet / Webbtester** ](app-insights-monitor-web-app-availability.md) -skicka regelbundna begäranden till ditt webbprogram runt world.*</span><span class="sxs-lookup"><span data-stu-id="3f0b7-189">[**Availability / Web tests**](app-insights-monitor-web-app-availability.md) - Send regular requests to your web app from around the world.*</span></span>
* <span data-ttu-id="3f0b7-190">[**Fel, prestanda** ](app-insights-web-monitor-performance.md) -undantag, fel och svarstider för förfrågningar från din app på förfrågningar till din app och [beroenden](app-insights-asp-net-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="3f0b7-190">[**Failures, Performance**](app-insights-web-monitor-performance.md) - Exceptions, failure rates and response times for requests to your app and for requests from your app to [dependencies](app-insights-asp-net-dependencies.md).</span></span>
* <span data-ttu-id="3f0b7-191">[**Prestanda** ](app-insights-web-monitor-performance.md) -svarstid, beroende svarstider.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-191">[**Performance**](app-insights-web-monitor-performance.md) - Response time, dependency response times.</span></span>
* <span data-ttu-id="3f0b7-192">[Servrar](app-insights-web-monitor-performance.md) -prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-192">[Servers](app-insights-web-monitor-performance.md) - Performance counters.</span></span> <span data-ttu-id="3f0b7-193">Tillgänglig om du [installera statusövervakaren](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="3f0b7-193">Available if you [install Status Monitor](app-insights-monitor-performance-live-website-now.md).</span></span>
* <span data-ttu-id="3f0b7-194">**Webbläsaren** -vy och AJAX-prestanda.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-194">**Browser** - Page view and AJAX performance.</span></span> <span data-ttu-id="3f0b7-195">Tillgänglig om du [instrumentera webbsidorna](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="3f0b7-195">Available if you [instrument your web pages](app-insights-javascript.md).</span></span>
* <span data-ttu-id="3f0b7-196">**Användning** -sidan Visa, användare och session räknas.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-196">**Usage** - Page view, user, and session counts.</span></span> <span data-ttu-id="3f0b7-197">Tillgänglig om du [instrumentera webbsidorna](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="3f0b7-197">Available if you [instrument your web pages](app-insights-javascript.md).</span></span>

<span data-ttu-id="3f0b7-198">KONFIGURERA</span><span class="sxs-lookup"><span data-stu-id="3f0b7-198">CONFIGURE</span></span>

* <span data-ttu-id="3f0b7-199">**Komma igång** -infogade kursen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-199">**Getting started** - inline tutorial.</span></span>
* <span data-ttu-id="3f0b7-200">**Egenskaper för** -instrumentation nyckel, prenumeration och resurs-id.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-200">**Properties** - instrumentation key, subscription and resource id.</span></span>
* <span data-ttu-id="3f0b7-201">[Aviseringar](app-insights-alerts.md) -mått varningskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-201">[Alerts](app-insights-alerts.md) - metric alert configuration.</span></span>
* <span data-ttu-id="3f0b7-202">[Löpande export](app-insights-export-telemetry.md) -konfigurera export av telemetri till Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-202">[Continuous export](app-insights-export-telemetry.md) - configure export of telemetry to Azure storage.</span></span>
* <span data-ttu-id="3f0b7-203">[Prestandatestning](app-insights-monitor-web-app-availability.md#performance-tests) -Ställ in en syntetisk belastningen på din webbplats.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-203">[Performance testing](app-insights-monitor-web-app-availability.md#performance-tests) - set up a synthetic load on your website.</span></span>
* <span data-ttu-id="3f0b7-204">[Kvoten och prissättning](app-insights-pricing.md) och [införandet provtagning](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="3f0b7-204">[Quota and pricing](app-insights-pricing.md) and [ingestion sampling](app-insights-sampling.md).</span></span>
* <span data-ttu-id="3f0b7-205">**API-åtkomst** -skapa [släpper anteckningar](app-insights-annotations.md) och Data Access-API: t.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-205">**API Access** - Create [release annotations](app-insights-annotations.md) and for the Data Access API.</span></span>
* <span data-ttu-id="3f0b7-206">[**Arbetsobjekt som** ](app-insights-diagnostic-search.md#create-work-item) -ansluta till ett system för uppföljning så att du kan skapa buggar vid undersökning av telemetri arbete.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-206">[**Work Items**](app-insights-diagnostic-search.md#create-work-item) - Connect to a work tracking system so that you can create bugs while inspecting telemetry.</span></span>

<span data-ttu-id="3f0b7-207">INSTÄLLNINGAR</span><span class="sxs-lookup"><span data-stu-id="3f0b7-207">SETTINGS</span></span>

* <span data-ttu-id="3f0b7-208">[**Låser** ](../azure-resource-manager/resource-group-lock-resources.md) -låsa Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="3f0b7-208">[**Locks**](../azure-resource-manager/resource-group-lock-resources.md) - lock Azure resources</span></span>
* <span data-ttu-id="3f0b7-209">[**Automatiseringsskriptet** ](app-insights-powershell.md) -exportera en definition av Azure-resurs så att du kan använda den som en mall för att skapa nya resurser.</span><span class="sxs-lookup"><span data-stu-id="3f0b7-209">[**Automation script**](app-insights-powershell.md) - export a definition of the Azure resource so that you can use it as a template to create new resources.</span></span>


## <a name="video"></a><span data-ttu-id="3f0b7-210">Video</span><span class="sxs-lookup"><span data-stu-id="3f0b7-210">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="3f0b7-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3f0b7-211">Next steps</span></span>

|  |  |
| --- | --- |
| [<span data-ttu-id="3f0b7-212">Metrics explorer</span><span class="sxs-lookup"><span data-stu-id="3f0b7-212">Metrics explorer</span></span>](app-insights-metrics-explorer.md)<br/><span data-ttu-id="3f0b7-213">Filter och segment mått</span><span class="sxs-lookup"><span data-stu-id="3f0b7-213">Filter and segment metrics</span></span> |![Sök-exempel](./media/app-insights-dashboards/64.png) |
| [<span data-ttu-id="3f0b7-215">Diagnostiska sökning</span><span class="sxs-lookup"><span data-stu-id="3f0b7-215">Diagnostic search</span></span>](app-insights-diagnostic-search.md)<br/><span data-ttu-id="3f0b7-216">Söka efter och granska händelser, relaterade händelser och skapa programfel</span><span class="sxs-lookup"><span data-stu-id="3f0b7-216">Find and inspect events, related events, and create bugs</span></span> |![Sök-exempel](./media/app-insights-dashboards/61.png) |
| [<span data-ttu-id="3f0b7-218">Analys</span><span class="sxs-lookup"><span data-stu-id="3f0b7-218">Analytics</span></span>](app-insights-analytics.md)<br/><span data-ttu-id="3f0b7-219">Kraftfulla frågespråket</span><span class="sxs-lookup"><span data-stu-id="3f0b7-219">Powerful query language</span></span> |![Sök-exempel](./media/app-insights-dashboards/63.png) |
