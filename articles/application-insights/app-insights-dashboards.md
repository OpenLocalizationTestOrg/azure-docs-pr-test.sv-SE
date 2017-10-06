---
title: aaaDashboards och navigering i hello Azure Application Insights | Microsoft Docs
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
ms.openlocfilehash: 58811388205643bb672e0405b3226f12d0f447a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="navigation-and-dashboards-in-hello-application-insights-portal"></a><span data-ttu-id="2b1a8-103">Navigering och instrumentpaneler i hello Application Insights-portalen</span><span class="sxs-lookup"><span data-stu-id="2b1a8-103">Navigation and Dashboards in hello Application Insights portal</span></span>
<span data-ttu-id="2b1a8-104">När du har [konfigurera Application Insights i ditt projekt](app-insights-overview.md), telemetridata om prestanda och användning av din app visas i Application Insights-resurs i ditt projekt i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2b1a8-104">After you have [set up Application Insights on your project](app-insights-overview.md), telemetry data about your app's performance and usage will appear in your project's Application Insights resource in hello [Azure portal](https://portal.azure.com).</span></span>

## <a name="find-your-telemetry"></a><span data-ttu-id="2b1a8-105">Hitta din telemetri</span><span class="sxs-lookup"><span data-stu-id="2b1a8-105">Find your telemetry</span></span>
<span data-ttu-id="2b1a8-106">Logga in toohello [Azure-portalen](https://portal.azure.com) och navigera toohello Application Insights-resurs som du skapade för din app.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-106">Sign in toohello [Azure portal](https://portal.azure.com) and navigate toohello Application Insights resource that you created for your app.</span></span>

![Klicka på Bläddra, Välj Application Insights och sedan din app.](./media/app-insights-dashboards/00-start.png)

<span data-ttu-id="2b1a8-108">hello översikt bladet (sidan) för din app visar en sammanfattning av hello diagnostiska nyckelvärden för din app och är en gateway-toohello andra funktioner i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-108">hello overview blade (page) for your app shows a summary of hello key diagnostic metrics of your app, and is a gateway toohello other features of hello portal.</span></span>

![Högre vägar tooview din telemetri](./media/app-insights-dashboards/010-oview.png)

<span data-ttu-id="2b1a8-110">Du kan anpassa någon av hello diagram och rutnät och fästa dem tooa instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-110">You can customize any of hello charts and grids and pin them tooa dashboard.</span></span> <span data-ttu-id="2b1a8-111">På så sätt kan du kan hämta hello viktiga telemetri från olika appar på en central instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-111">That way, you can bring together hello key telemetry from different apps on a central dashboard.</span></span>

## <a name="dashboards"></a><span data-ttu-id="2b1a8-112">Instrumentpaneler</span><span class="sxs-lookup"><span data-stu-id="2b1a8-112">Dashboards</span></span>
<span data-ttu-id="2b1a8-113">hello först öppna visas när du loggar in toohello [Microsoft Azure-portalen](https://portal.azure.com) är en instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-113">hello first thing you see after you sign in toohello [Microsoft Azure portal](https://portal.azure.com) is a dashboard.</span></span> <span data-ttu-id="2b1a8-114">Här kan du sammanfoga hello diagram som är viktigast tooyou för alla dina Azure-resurser, inklusive telemetri från [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2b1a8-114">Here you can bring together hello charts that are most important tooyou across all your Azure resources, including telemetry from [Azure Application Insights](app-insights-overview.md).</span></span>

![En anpassad instrumentpanel.](./media/app-insights-dashboards/31.png)

1. <span data-ttu-id="2b1a8-116">**Navigera toospecific resurser** som din app i Application Insights: Använd hello vänster stapel.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-116">**Navigate toospecific resources** such as your app in Application Insights: Use hello left bar.</span></span>
2. <span data-ttu-id="2b1a8-117">**Returnerar toohello den aktuella instrumentpanelen**, eller växla tooother senaste vyer: Använd hello listrutan längst upp till vänster.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-117">**Return toohello current dashboard**, or switch tooother recent views: Use hello drop-down menu at top left.</span></span>
3. <span data-ttu-id="2b1a8-118">**Växla instrumentpaneler**: Använd hello rullgardinsmenyn på hello instrumentpanelen rubrik</span><span class="sxs-lookup"><span data-stu-id="2b1a8-118">**Switch dashboards**: Use hello drop-down menu on hello dashboard title</span></span>
4. <span data-ttu-id="2b1a8-119">**Skapa, redigera och dela instrumentpaneler** i hello instrumentpanelen verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-119">**Create, edit, and share dashboards** in hello dashboard toolbar.</span></span>
5. <span data-ttu-id="2b1a8-120">**Redigera hello instrumentpanelen**: hovra över en panel och sedan använda överkanten liggande toomove, anpassa eller ta bort den.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-120">**Edit hello dashboard**: Hover over a tile and then use its top bar toomove, customize, or remove it.</span></span>

## <a name="add-tooa-dashboard"></a><span data-ttu-id="2b1a8-121">Lägga till tooa instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="2b1a8-121">Add tooa dashboard</span></span>
<span data-ttu-id="2b1a8-122">När du tittar på ett blad eller uppsättning diagram som är särskilt intressanta, kan du fästa en kopia av den toohello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-122">When you're looking at a blade or set of charts that's particularly interesting, you can pin a copy of it toohello dashboard.</span></span> <span data-ttu-id="2b1a8-123">Ser du den nästa gång du kommer tillbaka det.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-123">You'll see it next time you return there.</span></span>

![toopin ett diagram hovrar över det och klicka på ”...” i hello-huvudet.](./media/app-insights-dashboards/33.png)

1. <span data-ttu-id="2b1a8-125">Fästa diagrammet toodashboard.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-125">Pin chart toodashboard.</span></span> <span data-ttu-id="2b1a8-126">En kopia av hello schemat visas på hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-126">A copy of hello chart appears on hello dashboard.</span></span>
2. <span data-ttu-id="2b1a8-127">PIN-kod hello hela bladet toohello instrumentpanelen - visas den på hello instrumentpanel som en panel som du kan klicka på via.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-127">Pin hello whole blade toohello dashboard - it appears on hello dashboard as a tile that you can click through.</span></span>
3. <span data-ttu-id="2b1a8-128">Klicka på hello övre vänstra hörnet tooreturn toohello den aktuella instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-128">Click hello top left corner tooreturn toohello current dashboard.</span></span> <span data-ttu-id="2b1a8-129">Du kan använda hello nedrullningsbara menyn tooreturn toohello aktuell vy.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-129">Then you can use hello drop-down menu tooreturn toohello current view.</span></span>

<span data-ttu-id="2b1a8-130">Observera att diagram grupperas i panelerna: en panel kan innehålla fler än ett schema.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-130">Notice that charts are grouped into tiles: a tile can contain more than one chart.</span></span> <span data-ttu-id="2b1a8-131">Du kan fästa hello hela panelen toohello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-131">You pin hello whole tile toohello dashboard.</span></span>

<span data-ttu-id="2b1a8-132">hello diagrammet uppdateras automatiskt med en frekvens som är beroende av hello diagrammets tidsintervall:</span><span class="sxs-lookup"><span data-stu-id="2b1a8-132">hello chart is automatically refreshed with a frequency that depends on hello chart's time range:</span></span>

* <span data-ttu-id="2b1a8-133">Tidsintervallet in too1 timme: uppdatera var femte minut</span><span class="sxs-lookup"><span data-stu-id="2b1a8-133">Time range up too1 hour: Refresh every 5 minutes</span></span>
* <span data-ttu-id="2b1a8-134">Tidsintervallet 1-24 timmar: uppdatera var 15: e minut</span><span class="sxs-lookup"><span data-stu-id="2b1a8-134">Time range 1 - 24 hours: Refresh every 15 minutes</span></span>
* <span data-ttu-id="2b1a8-135">Tidsintervallet ovan 24 timmar: (tidsintervallet) / 60.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-135">Time range above 24 hours: (Time range)/60.</span></span>

### <a name="pin-any-query-in-analytics"></a><span data-ttu-id="2b1a8-136">Fästa en fråga i Analytics</span><span class="sxs-lookup"><span data-stu-id="2b1a8-136">Pin any query in Analytics</span></span>
<span data-ttu-id="2b1a8-137">Du kan också [fästa Analytics](app-insights-analytics-using.md#pin-to-dashboard) diagram tooa [delade](#share-dashboards-with-your-team) instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-137">You can also [pin Analytics](app-insights-analytics-using.md#pin-to-dashboard) charts tooa [shared](#share-dashboards-with-your-team) dashboard.</span></span> <span data-ttu-id="2b1a8-138">Detta ger dig tooadd diagram av en skadlig fråga tillsammans med hello standard mått.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-138">This allows you tooadd charts of any arbitrary query alongside hello standard metrics.</span></span> 

<span data-ttu-id="2b1a8-139">Resultaten beräknas automatiskt varje timme.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-139">Results are automatically recalculated every hour.</span></span> <span data-ttu-id="2b1a8-140">Klicka på hello-ikonen på hello diagram toorecalculate omedelbart.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-140">Click hello Refresh icon on hello chart toorecalculate immediately.</span></span> <span data-ttu-id="2b1a8-141">(Uppdatera webbläsaren omberäknar inte.)</span><span class="sxs-lookup"><span data-stu-id="2b1a8-141">(Browser refresh doesn't recalculate.)</span></span>

## <a name="adjust-a-tile-on-hello-dashboard"></a><span data-ttu-id="2b1a8-142">Justera en panel på instrumentpanelen för hello</span><span class="sxs-lookup"><span data-stu-id="2b1a8-142">Adjust a tile on hello dashboard</span></span>
<span data-ttu-id="2b1a8-143">Du kan justera när en panel på instrumentpanelen för hello.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-143">Once a tile is on hello dashboard, you can adjust it.</span></span>

![Hovra över ett diagram i ordning tooedit den.](./media/app-insights-dashboards/36.png)

1. <span data-ttu-id="2b1a8-145">Lägg till ett diagram toohello sida vid sida.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-145">Add a chart toohello tile.</span></span>
2. <span data-ttu-id="2b1a8-146">Ange hello mått, gruppera efter dimension och format (tabell, diagram) i ett diagram.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-146">Set hello metric, group-by dimension and style (table, graph) of a chart.</span></span>
3. <span data-ttu-id="2b1a8-147">Dra över hello diagram toozoom i; Klicka på hello Ångra knappen tooreset hello timespan; Ange filteregenskaper för hello diagram på hello sida vid sida.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-147">Drag across hello diagram toozoom in; click hello undo button tooreset hello timespan; set filter properties for hello charts on hello tile.</span></span>
4. <span data-ttu-id="2b1a8-148">Ange panelrubriken.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-148">Set tile title.</span></span>

<span data-ttu-id="2b1a8-149">Paneler fästs från mått explorer blad har fler redigera alternativ än paneler fästs från en översikt över bladet.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-149">Tiles pinned from metric explorer blades have more editing options than tiles pinned from an Overview blade.</span></span>

<span data-ttu-id="2b1a8-150">hello ursprungliga panelen som du har fäst påverkas inte av ändringarna.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-150">hello original tile that you pinned isn't affected by your edits.</span></span>

## <a name="switch-between-dashboards"></a><span data-ttu-id="2b1a8-151">Växla mellan instrumentpaneler</span><span class="sxs-lookup"><span data-stu-id="2b1a8-151">Switch between dashboards</span></span>
<span data-ttu-id="2b1a8-152">Du kan spara mer än en instrumentpanel och växla mellan dem.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-152">You can save more than one dashboard and switch between them.</span></span> <span data-ttu-id="2b1a8-153">När du fäster ett diagram eller bladet läggs de toohello den aktuella instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-153">When you pin a chart or blade, they're added toohello current dashboard.</span></span>

![tooswitch mellan instrumentpaneler, klicka på instrumentpanelen och välj en sparad instrumentpanel.](./media/app-insights-dashboards/32.png)

<span data-ttu-id="2b1a8-157">Du kan till exempel ha en instrumentpanel om du vill visa hela skärmen i hello team plats och en annan för allmänna utvecklingen.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-157">For example, you might have one dashboard for displaying full screen in hello team room, and another for general development.</span></span>

<span data-ttu-id="2b1a8-158">På instrumentpanelen hello visas ett blad som en panel: Klicka på den toogo toohello bladet.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-158">On hello dashboard, a blade appears as a tile: click it toogo toohello blade.</span></span> <span data-ttu-id="2b1a8-159">Ett diagram replikerar hello diagram på den ursprungliga platsen.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-159">A chart replicates hello chart in its original location.</span></span>

![Klicka på panelen tooopen hello bladet representerar](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a><span data-ttu-id="2b1a8-161">Dela instrumentpaneler</span><span class="sxs-lookup"><span data-stu-id="2b1a8-161">Share dashboards</span></span>
<span data-ttu-id="2b1a8-162">När du har skapat en instrumentpanel kan dela du den med andra användare.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-162">When you've created a dashboard, you can share it with other users.</span></span>

![Klicka på resurs i hello instrumentpanelen sidhuvud](./media/app-insights-dashboards/41.png)

<span data-ttu-id="2b1a8-164">Lär dig mer om [roller och åtkomstkontroll](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="2b1a8-164">Learn about [Roles and access control](app-insights-resources-roles-access-control.md).</span></span>

## <a name="app-navigation"></a><span data-ttu-id="2b1a8-165">App-navigering</span><span class="sxs-lookup"><span data-stu-id="2b1a8-165">App navigation</span></span>
<span data-ttu-id="2b1a8-166">hello översikt bladet är hello gateway toomore information om din app.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-166">hello overview blade is hello gateway toomore information about your app.</span></span>

* <span data-ttu-id="2b1a8-167">**Alla diagram eller panelen** – Klicka på panelen eller diagram toosee mer information om vad som visas.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-167">**Any chart or tile** - Click any tile or chart toosee more detail about what it displays.</span></span>

### <a name="overview-blade-buttons"></a><span data-ttu-id="2b1a8-168">Översikt över bladet knappar</span><span class="sxs-lookup"><span data-stu-id="2b1a8-168">Overview blade buttons</span></span>
![Översikt över bladet övre navigeringsfält](./media/app-insights-dashboards/app-overview-top-nav.png)

* <span data-ttu-id="2b1a8-170">[**Metrics Explorer** ](app-insights-metrics-explorer.md) -skapa egna diagram av prestanda och användning.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-170">[**Metrics Explorer**](app-insights-metrics-explorer.md) - Create your own charts of performance and usage.</span></span>
* <span data-ttu-id="2b1a8-171">[**Sök** ](app-insights-diagnostic-search.md) – undersöka specifika instanser av händelser, till exempel begäranden, undantag, eller logga in spårningar.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-171">[**Search**](app-insights-diagnostic-search.md) - Investigate specific instances of events such as requests, exceptions, or log traces.</span></span>
* <span data-ttu-id="2b1a8-172">[**Analytics** ](app-insights-analytics.md) -kraftfulla frågor via din telemetri.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-172">[**Analytics**](app-insights-analytics.md) - Powerful queries over your telemetry.</span></span>
* <span data-ttu-id="2b1a8-173">**Tidsintervallet** -justera hello-intervall som visas av alla hello diagram på hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-173">**Time range** - Adjust hello range displayed by all hello charts on hello blade.</span></span>
* <span data-ttu-id="2b1a8-174">**Ta bort** -ta bort hello Application Insights-resurs för den här appen.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-174">**Delete** - Delete hello Application Insights resource for this app.</span></span> <span data-ttu-id="2b1a8-175">Du bör också ta bort hello Application Insights paket från din Appkod, eller redigera hello [instrumentation nyckeln](app-insights-create-new-resource.md#copy-the-instrumentation-key) i din app toodirect telemetri tooa olika Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-175">You should also either remove hello Application Insights packages from your app code, or edit hello [instrumentation key](app-insights-create-new-resource.md#copy-the-instrumentation-key) in your app toodirect telemetry tooa different Application Insights resource.</span></span>

### <a name="essentials-tab"></a><span data-ttu-id="2b1a8-176">Fliken Essentials</span><span class="sxs-lookup"><span data-stu-id="2b1a8-176">Essentials tab</span></span>
* <span data-ttu-id="2b1a8-177">[Instrumentation nyckeln](app-insights-create-new-resource.md#copy-the-instrumentation-key) -identifierar den här appen resurs.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-177">[Instrumentation key](app-insights-create-new-resource.md#copy-the-instrumentation-key) - Identifies this app resource.</span></span>
* <span data-ttu-id="2b1a8-178">Priser för - och göra funktioner tillgängliga och ange volym versaler.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-178">Pricing - Make features available and set volume caps.</span></span>

### <a name="app-navigation-bar"></a><span data-ttu-id="2b1a8-179">Navigeringsfältet i appen</span><span class="sxs-lookup"><span data-stu-id="2b1a8-179">App navigation bar</span></span>
![Vänstra navigeringsfältet](./media/app-insights-dashboards/app-left-nav-bar.png)

* <span data-ttu-id="2b1a8-181">**Översikt över** -returnerade toohello app översikt bladet.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-181">**Overview** - Return toohello app overview blade.</span></span>
* <span data-ttu-id="2b1a8-182">**Aktivitetsloggen** -aviseringar och Azure administrativa händelser.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-182">**Activity log** - Alerts and Azure administrative events.</span></span>
* <span data-ttu-id="2b1a8-183">[**Åtkomstkontroll** ](app-insights-resources-roles-access-control.md) -ge åtkomst tooteam medlemmar med mera.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-183">[**Access control**](app-insights-resources-roles-access-control.md) - Provide access tooteam members and others.</span></span>
* <span data-ttu-id="2b1a8-184">[**Taggar** ](../azure-resource-manager/resource-group-using-tags.md) -Använd taggar toogroup din app med andra.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-184">[**Tags**](../azure-resource-manager/resource-group-using-tags.md) - Use tags toogroup your app with others.</span></span>

<span data-ttu-id="2b1a8-185">UNDERSÖK</span><span class="sxs-lookup"><span data-stu-id="2b1a8-185">INVESTIGATE</span></span>

* <span data-ttu-id="2b1a8-186">[**Programavbildningen** ](app-insights-app-map.md) -aktiv karta visar hello serverkomponenter i ditt program härlett från hello beroendeinformation.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-186">[**Application map**](app-insights-app-map.md) - Active map showing hello components of your application, derived from hello dependency information.</span></span>
* <span data-ttu-id="2b1a8-187">[**Identifiering för smartkort** ](app-insights-proactive-diagnostics.md) -granska de senaste prestanda aviseringarna.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-187">[**Smart Detection**](app-insights-proactive-diagnostics.md) - Review recent performance alerts.</span></span>
* <span data-ttu-id="2b1a8-188">[**Direktsänd dataström** ](app-insights-live-stream.md) – en fast uppsättning nästan omedelbar statistik, användbart när du distribuerar en ny version eller felsökning.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-188">[**Live Stream**](app-insights-live-stream.md) - A fixed set of near-instant metrics, useful when deploying a new build or debugging.</span></span>
* <span data-ttu-id="2b1a8-189">[**Tillgänglighet / Webbtester** ](app-insights-monitor-web-app-availability.md) -skickar regelbundet förfrågningar tooyour webbprogrammet från runt hello world.*</span><span class="sxs-lookup"><span data-stu-id="2b1a8-189">[**Availability / Web tests**](app-insights-monitor-web-app-availability.md) - Send regular requests tooyour web app from around hello world.*</span></span>
* <span data-ttu-id="2b1a8-190">[**Fel, prestanda** ](app-insights-web-monitor-performance.md) -undantag, fel och svar tidsgränsen för begäran tooyour app och förfrågningar från din app för[beroenden](app-insights-asp-net-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="2b1a8-190">[**Failures, Performance**](app-insights-web-monitor-performance.md) - Exceptions, failure rates and response times for requests tooyour app and for requests from your app too[dependencies](app-insights-asp-net-dependencies.md).</span></span>
* <span data-ttu-id="2b1a8-191">[**Prestanda** ](app-insights-web-monitor-performance.md) -svarstid, beroende svarstider.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-191">[**Performance**](app-insights-web-monitor-performance.md) - Response time, dependency response times.</span></span>
* <span data-ttu-id="2b1a8-192">[Servrar](app-insights-web-monitor-performance.md) -prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-192">[Servers](app-insights-web-monitor-performance.md) - Performance counters.</span></span> <span data-ttu-id="2b1a8-193">Tillgänglig om du [installera statusövervakaren](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="2b1a8-193">Available if you [install Status Monitor](app-insights-monitor-performance-live-website-now.md).</span></span>
* <span data-ttu-id="2b1a8-194">**Webbläsaren** -vy och AJAX-prestanda.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-194">**Browser** - Page view and AJAX performance.</span></span> <span data-ttu-id="2b1a8-195">Tillgänglig om du [instrumentera webbsidorna](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="2b1a8-195">Available if you [instrument your web pages](app-insights-javascript.md).</span></span>
* <span data-ttu-id="2b1a8-196">**Användning** -sidan Visa, användare och session räknas.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-196">**Usage** - Page view, user, and session counts.</span></span> <span data-ttu-id="2b1a8-197">Tillgänglig om du [instrumentera webbsidorna](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="2b1a8-197">Available if you [instrument your web pages](app-insights-javascript.md).</span></span>

<span data-ttu-id="2b1a8-198">KONFIGURERA</span><span class="sxs-lookup"><span data-stu-id="2b1a8-198">CONFIGURE</span></span>

* <span data-ttu-id="2b1a8-199">**Komma igång** -infogade kursen.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-199">**Getting started** - inline tutorial.</span></span>
* <span data-ttu-id="2b1a8-200">**Egenskaper för** -instrumentation nyckel, prenumeration och resurs-id.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-200">**Properties** - instrumentation key, subscription and resource id.</span></span>
* <span data-ttu-id="2b1a8-201">[Aviseringar](app-insights-alerts.md) -mått varningskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-201">[Alerts](app-insights-alerts.md) - metric alert configuration.</span></span>
* <span data-ttu-id="2b1a8-202">[Löpande export](app-insights-export-telemetry.md) -konfigurera export av telemetri tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-202">[Continuous export](app-insights-export-telemetry.md) - configure export of telemetry tooAzure storage.</span></span>
* <span data-ttu-id="2b1a8-203">[Prestandatestning](app-insights-monitor-web-app-availability.md#performance-tests) -Ställ in en syntetisk belastningen på din webbplats.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-203">[Performance testing](app-insights-monitor-web-app-availability.md#performance-tests) - set up a synthetic load on your website.</span></span>
* <span data-ttu-id="2b1a8-204">[Kvoten och prissättning](app-insights-pricing.md) och [införandet provtagning](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="2b1a8-204">[Quota and pricing](app-insights-pricing.md) and [ingestion sampling](app-insights-sampling.md).</span></span>
* <span data-ttu-id="2b1a8-205">**API-åtkomst** -skapa [släpper anteckningar](app-insights-annotations.md) och för hello Data Access-API.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-205">**API Access** - Create [release annotations](app-insights-annotations.md) and for hello Data Access API.</span></span>
* <span data-ttu-id="2b1a8-206">[**Arbetsobjekt som** ](app-insights-diagnostic-search.md#create-work-item) -ansluta tooa arbete system för uppföljning så att du kan skapa buggar vid undersökning av telemetri.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-206">[**Work Items**](app-insights-diagnostic-search.md#create-work-item) - Connect tooa work tracking system so that you can create bugs while inspecting telemetry.</span></span>

<span data-ttu-id="2b1a8-207">INSTÄLLNINGAR</span><span class="sxs-lookup"><span data-stu-id="2b1a8-207">SETTINGS</span></span>

* <span data-ttu-id="2b1a8-208">[**Låser** ](../azure-resource-manager/resource-group-lock-resources.md) -låsa Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="2b1a8-208">[**Locks**](../azure-resource-manager/resource-group-lock-resources.md) - lock Azure resources</span></span>
* <span data-ttu-id="2b1a8-209">[**Automatiseringsskriptet** ](app-insights-powershell.md) -exportera en definition av hello Azure-resurs så att du kan använda den som en mall toocreate nya resurser.</span><span class="sxs-lookup"><span data-stu-id="2b1a8-209">[**Automation script**](app-insights-powershell.md) - export a definition of hello Azure resource so that you can use it as a template toocreate new resources.</span></span>


## <a name="video"></a><span data-ttu-id="2b1a8-210">Video</span><span class="sxs-lookup"><span data-stu-id="2b1a8-210">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="2b1a8-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2b1a8-211">Next steps</span></span>

|  |  |
| --- | --- |
| [<span data-ttu-id="2b1a8-212">Metrics explorer</span><span class="sxs-lookup"><span data-stu-id="2b1a8-212">Metrics explorer</span></span>](app-insights-metrics-explorer.md)<br/><span data-ttu-id="2b1a8-213">Filter och segment mått</span><span class="sxs-lookup"><span data-stu-id="2b1a8-213">Filter and segment metrics</span></span> |![Sök-exempel](./media/app-insights-dashboards/64.png) |
| [<span data-ttu-id="2b1a8-215">Diagnostiska sökning</span><span class="sxs-lookup"><span data-stu-id="2b1a8-215">Diagnostic search</span></span>](app-insights-diagnostic-search.md)<br/><span data-ttu-id="2b1a8-216">Söka efter och granska händelser, relaterade händelser och skapa programfel</span><span class="sxs-lookup"><span data-stu-id="2b1a8-216">Find and inspect events, related events, and create bugs</span></span> |![Sök-exempel](./media/app-insights-dashboards/61.png) |
| [<span data-ttu-id="2b1a8-218">Analys</span><span class="sxs-lookup"><span data-stu-id="2b1a8-218">Analytics</span></span>](app-insights-analytics.md)<br/><span data-ttu-id="2b1a8-219">Kraftfulla frågespråket</span><span class="sxs-lookup"><span data-stu-id="2b1a8-219">Powerful query language</span></span> |![Sök-exempel](./media/app-insights-dashboards/63.png) |
