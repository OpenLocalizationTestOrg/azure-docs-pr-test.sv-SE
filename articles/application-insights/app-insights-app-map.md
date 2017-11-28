---
title: aaaApplication kartan i Azure Application Insights | Microsoft Docs
description: "En visuell presentation av hello beroenden mellan komponenter för appar som är märkta med KPI: er och -varningar."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 96ab753a100ea53ec7d367e3559b6622ab6fd182
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-map-in-application-insights"></a><span data-ttu-id="f6a0e-103">Programavbildningen i Application Insights</span><span class="sxs-lookup"><span data-stu-id="f6a0e-103">Application Map in Application Insights</span></span>
<span data-ttu-id="f6a0e-104">I [Azure Application Insights](app-insights-overview.md), programavbildningen är en visuell layout av hello beroenden av programkomponenterna.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-104">In [Azure Application Insights](app-insights-overview.md), Application Map is a visual layout of hello dependency relationships of your application components.</span></span> <span data-ttu-id="f6a0e-105">Varje komponent visas KPI: er, till exempel belastning, prestanda, fel och varningar, toohelp du identifiera någon komponent som orsakar ett prestandaproblem eller fel.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-105">Each component shows KPIs such as load, performance, failures, and alerts, toohelp you discover any component causing a performance issue or failure.</span></span> <span data-ttu-id="f6a0e-106">Du kan klicka på via från någon komponent toomore detaljerad diagnostik, till exempel Application Insights händelser.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-106">You can click through from any component toomore detailed diagnostics, such as Application Insights events.</span></span> <span data-ttu-id="f6a0e-107">Om din app använder Azure-tjänster, kan du också klicka tooAzure diagnostiken, till exempel SQL Database Advisor-rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-107">If your app uses Azure services, you can also click through tooAzure diagnostics, such as SQL Database Advisor recommendations.</span></span>

<span data-ttu-id="f6a0e-108">Du kan fästa ett program kartan toohello Azure instrumentpanel, där det är fullt fungerande som andra diagram.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-108">Like other charts, you can pin an application map toohello Azure dashboard, where it is fully functional.</span></span> 

## <a name="open-hello-application-map"></a><span data-ttu-id="f6a0e-109">Öppna hello programavbildningen</span><span class="sxs-lookup"><span data-stu-id="f6a0e-109">Open hello application map</span></span>
<span data-ttu-id="f6a0e-110">Öppna hello mappning från hello översikt bladet för ditt program:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-110">Open hello map from hello overview blade for your application:</span></span>

![Öppna appen karta](./media/app-insights-app-map/01.png)

![App-karta](./media/app-insights-app-map/02.png)

<span data-ttu-id="f6a0e-113">hello kartan visar:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-113">hello map shows:</span></span>

* <span data-ttu-id="f6a0e-114">Tillgänglighetstester</span><span class="sxs-lookup"><span data-stu-id="f6a0e-114">Availability tests</span></span>
* <span data-ttu-id="f6a0e-115">Klient-komponenten (övervakas med hello JavaScript SDK)</span><span class="sxs-lookup"><span data-stu-id="f6a0e-115">Client-side component (monitored with hello JavaScript SDK)</span></span>
* <span data-ttu-id="f6a0e-116">Server-sida-komponent</span><span class="sxs-lookup"><span data-stu-id="f6a0e-116">Server-side component</span></span>
* <span data-ttu-id="f6a0e-117">Beroenden för hello klient- och serverkomponenter</span><span class="sxs-lookup"><span data-stu-id="f6a0e-117">Dependencies of hello client and server components</span></span>

<span data-ttu-id="f6a0e-118">Du kan expandera och komprimera beroende länken grupper:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-118">You can expand and collapse dependency link groups:</span></span>

![Dölj](./media/app-insights-app-map/03.png)

<span data-ttu-id="f6a0e-120">Om du har många beroenden av en typ (SQL, http-etc.), kan de visas grupperade.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-120">If you have many dependencies of one type (SQL, HTTP etc.), they may appear grouped.</span></span> 

![grupperade beroenden](./media/app-insights-app-map/03-2.png)

## <a name="spot-problems"></a><span data-ttu-id="f6a0e-122">Hitta problem</span><span class="sxs-lookup"><span data-stu-id="f6a0e-122">Spot problems</span></span>
<span data-ttu-id="f6a0e-123">Varje nod har relevanta nyckeltal, till exempel hello belastning, prestanda och fel priser för den komponenten.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-123">Each node has relevant performance indicators, such as hello load, performance, and failure rates for that component.</span></span> 

<span data-ttu-id="f6a0e-124">Varning ikoner Markera möjliga problem.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-124">Warning icons highlight possible problems.</span></span> <span data-ttu-id="f6a0e-125">En orange varning som innebär att det finns fel i förfrågningar, sidvisningar eller beroendeanrop.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-125">An orange warning means there are failures in requests, page views or dependency calls.</span></span> <span data-ttu-id="f6a0e-126">Rött innebär en felintervall över 5 procent.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-126">Red means a failure rate above 5%.</span></span> <span data-ttu-id="f6a0e-127">Om du vill tooadjust tröskelvärdena, öppna alternativ.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-127">If you want tooadjust these thresholds, open Options.</span></span>

![fel ikoner](./media/app-insights-app-map/04.png)

<span data-ttu-id="f6a0e-129">Aktiva aviseringar också visa upp:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-129">Active alerts also show up:</span></span> 

![aktiva aviseringar](./media/app-insights-app-map/05.png)

<span data-ttu-id="f6a0e-131">Om du använder SQL Azure, det finns en ikon som visas när det finns rekommendationer om hur du kan förbättra prestanda.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-131">If you use SQL Azure, there's an icon that shows when there are recommendations on how you can improve performance.</span></span> 

![Azure rekommendation](./media/app-insights-app-map/06.png)

<span data-ttu-id="f6a0e-133">Klicka på en ikon tooget mer information:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-133">Click any icon tooget more details:</span></span>

![Azure rekommendation](./media/app-insights-app-map/07.png)

## <a name="diagnostic-click-through"></a><span data-ttu-id="f6a0e-135">Diagnostiska klicka dig igenom</span><span class="sxs-lookup"><span data-stu-id="f6a0e-135">Diagnostic click through</span></span>
<span data-ttu-id="f6a0e-136">Varje hello nod på hello karta erbjuder riktade klicka dig igenom för diagnostik.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-136">Each of hello nodes on hello map offers targeted click through for diagnostics.</span></span> <span data-ttu-id="f6a0e-137">hello alternativen varierar beroende på hello typ av hello-nod.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-137">hello options vary depending on hello type of hello node.</span></span>

![Serveralternativ](./media/app-insights-app-map/09.png)

<span data-ttu-id="f6a0e-139">För komponenter som finns i Azure, bland hello Direktlänkar toothem.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-139">For components that are hosted in Azure, hello options include direct links toothem.</span></span>

## <a name="filters-and-time-range"></a><span data-ttu-id="f6a0e-140">Filter och tidsintervall</span><span class="sxs-lookup"><span data-stu-id="f6a0e-140">Filters and time range</span></span>
<span data-ttu-id="f6a0e-141">Som standard sammanfattas hello Mappa alla hello data som är tillgängliga för hello valt tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-141">By default, hello map summarizes all hello data available for hello chosen time range.</span></span> <span data-ttu-id="f6a0e-142">Men du kan filtrera tooinclude specifika åtgärden namn eller beroenden.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-142">But you can filter it tooinclude only specific operation names or dependencies.</span></span>

* <span data-ttu-id="f6a0e-143">Åtgärdsnamn: Detta inkluderar både sidvisningar och typer av begäranden på serversidan.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-143">Operation name: This includes both page views and server-side request types.</span></span> <span data-ttu-id="f6a0e-144">Med det här alternativet hello hello karta visar KPI på hello server-klientsidan nod för valda hello-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-144">With this option, hello map shows hello KPI on hello server/client-side node for hello selected operations only.</span></span> <span data-ttu-id="f6a0e-145">Den visar hello beroenden anropas i kontexten hello över de specifika åtgärder.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-145">It shows hello dependencies called in hello context of those specific operations.</span></span>
* <span data-ttu-id="f6a0e-146">Beroende basnamn: Detta inkluderar hello AJAX webbläsare beroenden och beroenden för serversidan.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-146">Dependency base name: This includes hello AJAX browser dependencies and server-side dependencies.</span></span> <span data-ttu-id="f6a0e-147">Om du rapporterar anpassade beroendetelemetri med hello TrackDependency API visas de också här.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-147">If you report custom dependency telemetry with hello TrackDependency API, they also appear here.</span></span> <span data-ttu-id="f6a0e-148">Du kan välja hello beroenden tooshow på hello karta.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-148">You can select hello dependencies tooshow on hello map.</span></span> <span data-ttu-id="f6a0e-149">Det här urvalet filtrera för närvarande inte hello serversidan förfrågningar eller hello klientsidan sidvisningar.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-149">Currently this selection does not filter hello server-side requests, or hello client-side page views.</span></span>

![Ange filter](./media/app-insights-app-map/11.png)

## <a name="save-filters"></a><span data-ttu-id="f6a0e-151">Spara filter</span><span class="sxs-lookup"><span data-stu-id="f6a0e-151">Save filters</span></span>
<span data-ttu-id="f6a0e-152">toosave hello filter som du har använt, PIN-kod hello filtrerad vy på en [instrumentpanelen](app-insights-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="f6a0e-152">toosave hello filters you have applied, pin hello filtered view onto a [dashboard](app-insights-dashboards.md).</span></span>

![PIN-kod toodashboard](./media/app-insights-app-map/12.png)

## <a name="error-pane"></a><span data-ttu-id="f6a0e-154">Fel-fönstret</span><span class="sxs-lookup"><span data-stu-id="f6a0e-154">Error pane</span></span>
<span data-ttu-id="f6a0e-155">När du klickar på en nod i hello karta visas ett fel fönstret hello höger sammanfattning fel för noden.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-155">When you click a node in hello map, an error pane is displayed on hello right-hand side summarizing failures for that node.</span></span> <span data-ttu-id="f6a0e-156">Fel grupperas först efter åtgärds-ID och sedan grupperade efter problem-ID.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-156">Failures are grouped first by operation ID and then grouped by problem ID.</span></span>

![Fel-fönstret](./media/app-insights-app-map/error-pane.png)

<span data-ttu-id="f6a0e-158">Klicka på ett fel går du toohello senaste förekomst av att fel.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-158">Clicking on a failure takes you toohello most recent instance of that failure.</span></span>

## <a name="resource-health"></a><span data-ttu-id="f6a0e-159">Resurshälsa</span><span class="sxs-lookup"><span data-stu-id="f6a0e-159">Resource health</span></span>
<span data-ttu-id="f6a0e-160">För vissa typer av resurser visas resurshälsa hello överst i fönstret för hello-fel.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-160">For some resource types, resource health is displayed at hello top of hello error pane.</span></span> <span data-ttu-id="f6a0e-161">Till exempel visas klickar på en SQL-nod hello databasen hälsa och eventuella aviseringar som har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-161">For example, clicking a SQL node will show hello database health and any alerts that have fired.</span></span>

![Resurshälsa](./media/app-insights-app-map/resource-health.png)

<span data-ttu-id="f6a0e-163">Du kan klicka på hello resurs namn tooview standard översikt måtten för resursen.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-163">You can click hello resource name tooview standard overview metrics for that resource.</span></span>

## <a name="end-to-end-system-app-maps"></a><span data-ttu-id="f6a0e-164">Slutpunkt till slutpunkt system app maps</span><span class="sxs-lookup"><span data-stu-id="f6a0e-164">End-to-end system app maps</span></span>

<span data-ttu-id="f6a0e-165">*Kräver SDK version 2.3 eller senare*</span><span class="sxs-lookup"><span data-stu-id="f6a0e-165">*Requires SDK version 2.3 or higher*</span></span>

<span data-ttu-id="f6a0e-166">Om programmet har flera komponenter – till exempel en backend tjänst dessutom toohello webbapp – och du kan visa dem på en inbyggd app-karta.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-166">If your application has several components - for example, a back-end service in addition toohello web app - then you can show them all on one integrated app map.</span></span>

![Ange filter](./media/app-insights-app-map/multi-component-app-map.png)

<span data-ttu-id="f6a0e-168">hello app kartan hittar servernoder genom att följa alla HTTP-beroendeanrop mellan servrar med hello Application Insights SDK installerad.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-168">hello app map finds server nodes by following any HTTP dependency calls made between servers with hello Application Insights SDK installed.</span></span> <span data-ttu-id="f6a0e-169">Varje Application Insights-resurs antas toocontain en server.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-169">Each Application Insights resource is assumed toocontain one server.</span></span>

### <a name="multi-role-app-map-preview"></a><span data-ttu-id="f6a0e-170">Flera rollen app karta (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="f6a0e-170">Multi-role app map (preview)</span></span>

<span data-ttu-id="f6a0e-171">hello förhandsgranskningsfunktion flera rollen app kartan kan du toouse hello app karta med flera servrar som skickar data toohello samma Application Insights-resurs / instrumentation nyckel.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-171">hello preview multi-role app map feature allows you toouse hello app map with multiple servers sending data toohello same Application Insights resource  / instrumentation key.</span></span> <span data-ttu-id="f6a0e-172">Servrar i hello kartan segmenterade av hello cloud_RoleName-egenskapen på telemetri objekt.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-172">Servers in hello map are segmented by hello cloud_RoleName property on telemetry items.</span></span> <span data-ttu-id="f6a0e-173">Ange *flera rollen programavbildningen* för*på* från hello förhandsgranskningar bladet tooenable den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-173">Set *Multi-role Application Map* too*On* from hello Previews blade tooenable this configuration.</span></span>

<span data-ttu-id="f6a0e-174">Den här metoden kan det vara önskvärt i ett micro-services-program, eller i andra scenarier där du vill ha toocorrelate händelser över flera servrar i en enda Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-174">This approach may be desired in a micro-services application, or in other scenarios where you want toocorrelate events across multiple servers within a single Application Insights resource.</span></span>

## <a name="video"></a><span data-ttu-id="f6a0e-175">Video</span><span class="sxs-lookup"><span data-stu-id="f6a0e-175">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a><span data-ttu-id="f6a0e-176">Feedback</span><span class="sxs-lookup"><span data-stu-id="f6a0e-176">Feedback</span></span>
<span data-ttu-id="f6a0e-177">Lämna feedback via hello portal feedback.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-177">Please provide feedback through hello portal feedback option.</span></span>

![MapLink-1-bild](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a><span data-ttu-id="f6a0e-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f6a0e-179">Next steps</span></span>

* [<span data-ttu-id="f6a0e-180">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f6a0e-180">Azure portal</span></span>](https://portal.azure.com)
