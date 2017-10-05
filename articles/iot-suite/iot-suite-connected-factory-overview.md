---
title: "Översikt över Ansluten fabrik i Microsoft Azure IoT Suite | Microsoft Docs"
description: "En beskrivning av den förkonfigurerade lösningen Ansluten fabrik i Azure IoT Suite."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: d502c8e2e2715899279f6ebcf7ed89c19a1bb9a6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-the-connected-factory-preconfigured-solution"></a><span data-ttu-id="61870-103">Kom igång med den förkonfigurerade lösningen Ansluten fabrik</span><span class="sxs-lookup"><span data-stu-id="61870-103">Get started with the connected factory preconfigured solution</span></span>

<span data-ttu-id="61870-104">Azure IoT Suites [förkonfigurerade lösningar][lnk-preconfigured-solutions] kombinerar flera Azure IoT-tjänster för att leverera lösningar från slutpunkt till slutpunkt som implementerar vanliga IoT-företagsscenarier.</span><span class="sxs-lookup"><span data-stu-id="61870-104">Azure IoT Suite [preconfigured solutions][lnk-preconfigured-solutions] combine multiple Azure IoT services to deliver end-to-end solutions that implement common IoT business scenarios.</span></span> <span data-ttu-id="61870-105">Den förkonfigurerade lösningen *Ansluten fabrik* ansluter till och övervakar dina industriella enheter.</span><span class="sxs-lookup"><span data-stu-id="61870-105">The *connected factory* preconfigured solution connects to and monitors your industrial devices.</span></span> <span data-ttu-id="61870-106">Med lösningen kan du analysera dataströmmar från dina enheter och öka produktiviteten och lönsamheten i verksamheten.</span><span class="sxs-lookup"><span data-stu-id="61870-106">You can use the solution to analyze the stream of data from your devices and to drive operational productivity and profitability.</span></span>

<span data-ttu-id="61870-107">Den här självstudiekursen visar hur du etablerar den förkonfigurerade lösningen Ansluten fabrik.</span><span class="sxs-lookup"><span data-stu-id="61870-107">This tutorial shows you how to provision the connected factory preconfigured solution.</span></span> <span data-ttu-id="61870-108">Vi går också igenom de grundläggande funktionerna i den förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-108">It also walks you through the basic features of the preconfigured solution.</span></span> <span data-ttu-id="61870-109">Du kan komma åt många av dessa funktioner från *instrumentpanelen* för lösningen som distribueras som en del av den förkonfigurerade lösningen:</span><span class="sxs-lookup"><span data-stu-id="61870-109">You can access many of these features from the solution *dashboard* that deploys as part of the preconfigured solution:</span></span>

![instrumentpanel för den förkonfigurerade lösningen ansluten fabrik][img-cf-home]

<span data-ttu-id="61870-111">Du behöver en aktiv Azure-prenumeration för att kunna utföra stegen i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="61870-111">To complete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="61870-112">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="61870-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="61870-113">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="61870-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
> 
> 

## <a name="provision-the-solution"></a><span data-ttu-id="61870-114">Etablera lösningen</span><span class="sxs-lookup"><span data-stu-id="61870-114">Provision the solution</span></span>

1. <span data-ttu-id="61870-115">Logga in på azureiotsuite.com med din Azure-kontoinformation och skapa en lösning genom att klicka på **+**.</span><span class="sxs-lookup"><span data-stu-id="61870-115">Log on to azureiotsuite.com using your Azure account credentials, and click "**+**" to create a solution.</span></span>
2. <span data-ttu-id="61870-116">Klicka på **Välj** på panelen **Ansluten fabrik**.</span><span class="sxs-lookup"><span data-stu-id="61870-116">Click **Select** on the **Connected factory** tile.</span></span>
3. <span data-ttu-id="61870-117">Ange ett **lösningsnamn** för den förkonfigurerade anslutna fabriken för fjärrövervakning.</span><span class="sxs-lookup"><span data-stu-id="61870-117">Enter a **Solution name** for your connected factory preconfigured solution.</span></span>
4. <span data-ttu-id="61870-118">Välj den **prenumeration** och **region** som du vill använda för att etablera lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-118">Select the **Subscription** and **Region** you want to use to provision the solution.</span></span>
5. <span data-ttu-id="61870-119">Klicka på **Skapa lösning** för att påbörja etableringen.</span><span class="sxs-lookup"><span data-stu-id="61870-119">Click **Create Solution** to begin the provisioning process.</span></span> <span data-ttu-id="61870-120">Den här processen tar normalt flera minuter.</span><span class="sxs-lookup"><span data-stu-id="61870-120">This process typically takes several minutes to run.</span></span>

### <a name="while-you-wait-for-the-provisioning-process-to-complete"></a><span data-ttu-id="61870-121">Medan du väntar på att etableringsprocessen ska slutföras</span><span class="sxs-lookup"><span data-stu-id="61870-121">While you wait for the provisioning process to complete</span></span>

1. <span data-ttu-id="61870-122">Klicka på ikonen för din lösning med statusen **Etablerar**.</span><span class="sxs-lookup"><span data-stu-id="61870-122">Click the tile for your solution with **Provisioning** status.</span></span>
2. <span data-ttu-id="61870-123">Observera **etableringsstatusen** när Azure-tjänsterna distribueras i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="61870-123">Notice the **Provisioning states** as Azure services are deployed in your Azure subscription.</span></span>
3. <span data-ttu-id="61870-124">När etableringen har slutförts ändras statusen till **Klar**.</span><span class="sxs-lookup"><span data-stu-id="61870-124">Once provisioning completes, the status changes to **Ready**.</span></span>
4. <span data-ttu-id="61870-125">Klicka på ikonen så ser du informationen om din lösning i den högra rutan.</span><span class="sxs-lookup"><span data-stu-id="61870-125">Click the tile to see the details of your solution in the right-hand pane.</span></span>

> [!NOTE]
> <span data-ttu-id="61870-126">Om det uppstår problem när du distribuerar den förkonfigurerade lösningen kan du läsa [Behörigheter på webbplatsen azureiotsuite.com][lnk-permissions] och [Vanliga frågor och svar om ansluten fabrik](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="61870-126">If you encounter issues deploying the preconfigured solution, review [Permissions on the azureiotsuite.com site][lnk-permissions] and the [Connected factory FAQ](iot-suite-faq-cf.md).</span></span> <span data-ttu-id="61870-127">Om problemen kvarstår så skapa en tjänstbiljett på [portalen][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="61870-127">If the issues persist, create a service ticket on the [portal][lnk-portal].</span></span>

<span data-ttu-id="61870-128">Finns det något som du förväntar dig att se men som inte visas för din lösning?</span><span class="sxs-lookup"><span data-stu-id="61870-128">Are there details you'd expect to see that aren't listed for your solution?</span></span> <span data-ttu-id="61870-129">Lämna förslag på funktioner i [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span><span class="sxs-lookup"><span data-stu-id="61870-129">Give us feature suggestions on [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="61870-130">Scenarioöversikt</span><span class="sxs-lookup"><span data-stu-id="61870-130">Scenario overview</span></span>

<span data-ttu-id="61870-131">När du distribuerar den förkonfigurerade lösningen Ansluten fabrik innehåller den redan resurser som hjälper dig att gå igenom ett vanligt industriscenario.</span><span class="sxs-lookup"><span data-stu-id="61870-131">When you deploy the connected factory preconfigured solution, it is prepopulated with resources that enable you to step through a common industrial scenario.</span></span> <span data-ttu-id="61870-132">I det här scenariot rapporterar flera fabriker (som är anslutna till lösningen) de datavärden som krävs för att beräkna utrustningseffektivitet (Overall Equipment Efficiency, OEE) och KPI: er (Key Performance Indicators).</span><span class="sxs-lookup"><span data-stu-id="61870-132">In this scenario, several factories connected to the solution report the data values required to compute overall equipment efficiency (OEE) and key performance indicators (KPIs).</span></span> <span data-ttu-id="61870-133">I avsnitten nedan får du information om följande:</span><span class="sxs-lookup"><span data-stu-id="61870-133">The following sections show you how to:</span></span>

* <span data-ttu-id="61870-134">Övervaka fabrik, produktionslinjer, station OEE och KPI-värden</span><span class="sxs-lookup"><span data-stu-id="61870-134">Monitor factory, production lines, station OEE, and KPI values</span></span>
* <span data-ttu-id="61870-135">Analysera telemetridata som genereras av dessa enheter med hjälp av Azure Time Series Insights</span><span class="sxs-lookup"><span data-stu-id="61870-135">Analyze the telemetry data generated from these devices using Azure Time Series Insights</span></span>
* <span data-ttu-id="61870-136">Åtgärda problem vid aviseringar</span><span class="sxs-lookup"><span data-stu-id="61870-136">Act on alerts to fix issues</span></span>

<span data-ttu-id="61870-137">En viktig egenskap i detta scenario är att du kan utföra alla dessa åtgärder via fjärranslutning från instrumentpanelen med lösningar.</span><span class="sxs-lookup"><span data-stu-id="61870-137">A key feature of this scenario is that you can perform all these actions remotely from the solution dashboard.</span></span> <span data-ttu-id="61870-138">Du behöver inte fysisk åtkomst till enheterna.</span><span class="sxs-lookup"><span data-stu-id="61870-138">You do not need physical access to the devices.</span></span>

## <a name="view-the-solution-dashboard"></a><span data-ttu-id="61870-139">Visa instrumentpanelen för lösningen</span><span class="sxs-lookup"><span data-stu-id="61870-139">View the solution dashboard</span></span>

<span data-ttu-id="61870-140">På instrumentpanelen för lösningen kan du hantera den distribuerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-140">The solution dashboard enables you to manage the deployed solution.</span></span> <span data-ttu-id="61870-141">Det här är en hierarkisk representation av en global fabrikskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="61870-141">It is a hierarchical representation of a global factory configuration.</span></span> <span data-ttu-id="61870-142">Du kan till exempel se OEE och KPI:er, publicera nya noder för telemetri och åtgärdsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="61870-142">For example, you can view OEE and KPIs, publish new nodes for telemetry and action alerts.</span></span>

1. <span data-ttu-id="61870-143">När etableringen har slutförts och panelen för din förkonfigurerade lösning visar statusen **Klar** klickar du på **Starta**. Därmed öppnas portalen för lösningen Ansluten fabrik på en ny flik.</span><span class="sxs-lookup"><span data-stu-id="61870-143">When the provisioning is complete and the tile for your preconfigured solution indicates **Ready**, choose **Launch** to open your connected factory solution portal in a new tab.</span></span>

    ![Starta den förkonfigurerade lösningen][img-launch-solution]

1. <span data-ttu-id="61870-145">Som standard visar lösningsportalen *instrumentpanelen*.</span><span class="sxs-lookup"><span data-stu-id="61870-145">By default, the solution portal shows the *dashboard*.</span></span> <span data-ttu-id="61870-146">Du kan navigera till andra delar av portalen via menyn till vänster på sidan.</span><span class="sxs-lookup"><span data-stu-id="61870-146">To navigate to other areas of the portal, use the menu on the left-hand side of the page.</span></span>

    ![Instrumentpanel för den förkonfigurerade lösningen Ansluten fabrik][cf-img-menu]

<span data-ttu-id="61870-148">Följande information visas på instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="61870-148">The dashboard displays the following information:</span></span>

* <span data-ttu-id="61870-149">En panel med en **fabrikslista** som visar status, plats och aktuell produktionskonfiguration i lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-149">A **Factory list** panel that shows the status, location, and current production configuration in the solution.</span></span> <span data-ttu-id="61870-150">Första gången du kör lösningen finns det fyra simulerade enheter.</span><span class="sxs-lookup"><span data-stu-id="61870-150">When you first run the solution, there are a number of simulated devices.</span></span> <span data-ttu-id="61870-151">Produktionslinjesimuleringen består av tre verkliga OPC UA-servrar per produktionslinje. Dessa utför simulerade uppgifter och delar data.</span><span class="sxs-lookup"><span data-stu-id="61870-151">The production line simulation is composed of three real OPC UA servers per production line that perform simulated tasks and share data.</span></span> <span data-ttu-id="61870-152">Mer information om OPC UA finns i [Vanliga frågor och svar om ansluten fabrik](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="61870-152">For more information about OPC UA, see the [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>
* <span data-ttu-id="61870-153">En **karta** visar platsen för alla enheter som är kopplade till lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-153">A **map** that displays the location of each device connected to the solution.</span></span> <span data-ttu-id="61870-154">Lösningen kan använda Bing Maps-API:t för att rita information på kartan.</span><span class="sxs-lookup"><span data-stu-id="61870-154">The solution can use the Bing Maps API to plot information on the map.</span></span> <span data-ttu-id="61870-155">Om Bing Maps Enterprise API är aktiverat för din prenumeration används den här funktionen automatiskt.</span><span class="sxs-lookup"><span data-stu-id="61870-155">If your subscription is enabled for Bing Maps Enterprise API, then this feature is used automatically.</span></span> <span data-ttu-id="61870-156">I annat fall kan du se [vanliga frågor och svar][lnk-faq] för information om hur du gör kartan dynamisk.</span><span class="sxs-lookup"><span data-stu-id="61870-156">If not, see the [FAQ][lnk-faq] to learn how to make the map dynamic.</span></span>
* <span data-ttu-id="61870-157">En panel för **aviseringar** som visar aviseringar som genereras när ett telemetri- eller OEE/KPI-värde överskrider ett visst tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="61870-157">An **Alerts** panel that displays alerts generated when a telemetry or OEE/KPI value exceeds a specific threshold.</span></span>
* <span data-ttu-id="61870-158">En panel för **OEE** (Overall Equipment Efficiency) som visar OEE-värdena för hela företaget eller den fabrik, produktionslinje eller station du tittar på.</span><span class="sxs-lookup"><span data-stu-id="61870-158">An **Overall Equipment Efficiency** panel that shows the OEE values for the whole enterprise, or the factory/production line/station you are viewing.</span></span> <span data-ttu-id="61870-159">Det här värdet sammanställs från stationsvyn till företagsnivån.</span><span class="sxs-lookup"><span data-stu-id="61870-159">This value is aggregated from the station view to the enterprise level.</span></span> <span data-ttu-id="61870-160">OEE-bilden och dess beståndsdelar kan analyseras ytterligare.</span><span class="sxs-lookup"><span data-stu-id="61870-160">The OEE figure and its constituent elements can be further analyzed.</span></span>
* <span data-ttu-id="61870-161">Panelen för **KPI:er** som visar antalet enheter som producerats och energi som förbrukas av hela företaget eller den fabrik, produktionslinje eller station som du visar.</span><span class="sxs-lookup"><span data-stu-id="61870-161">**Key Performance Indicators** panel that displays the number of units produced and energy used by the whole enterprise or the factory/production line/station you are viewing.</span></span> <span data-ttu-id="61870-162">Dessa värden sammanställs från stationsvyn till företagsnivån.</span><span class="sxs-lookup"><span data-stu-id="61870-162">These values are aggregated from a station view to the enterprise level.</span></span>

## <a name="view-factories"></a><span data-ttu-id="61870-163">Visa fabriker</span><span class="sxs-lookup"><span data-stu-id="61870-163">View factories</span></span>

<span data-ttu-id="61870-164">Panelen för *fabriker* visar den geografiska platsen för alla fabriker i lösningen, deras status och den aktuella produktionskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="61870-164">The *Factories* panel shows you the geographical location of all the factories in the solution, their status, and current production configuration.</span></span> <span data-ttu-id="61870-165">Från listan över platser kan du navigera till andra nivåer i lösningshierarkin.</span><span class="sxs-lookup"><span data-stu-id="61870-165">From the locations list, you can navigate to the other levels in the solution hierarchy.</span></span> <span data-ttu-id="61870-166">Raderna i listan är hyperlänkar till information om produktionslinjerna på den platsen.</span><span class="sxs-lookup"><span data-stu-id="61870-166">The rows in the list are hyperlinks that link details of the production lines at that location.</span></span> <span data-ttu-id="61870-167">Du kan söka vidare i informationen om produktionslinjen ned till vyn på stationsnivå.</span><span class="sxs-lookup"><span data-stu-id="61870-167">It is then possible to drill into the production line details and down to the station level view.</span></span> <span data-ttu-id="61870-168">Du kan också använda ett filter för listan.</span><span class="sxs-lookup"><span data-stu-id="61870-168">You can also apply a filter to the list.</span></span>

![Fabriker för den förkonfigurerade lösningen Ansluten fabrik][cf-img-factories] 

1. <span data-ttu-id="61870-170">På **fabrikspanelen** visas fabrikslistan för lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-170">The **Factory panel** shows the factory list for this solution.</span></span>

2. <span data-ttu-id="61870-171">I fabrikslistan visas inledningsvis sex fabriker som skapades vid etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="61870-171">The factory list initially shows six factories created by the provisioning process.</span></span> <span data-ttu-id="61870-172">Du kan lägga till ytterligare simulerade och fysiska enheter till lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-172">You can add additional simulated and physical devices to the solution.</span></span>

3. <span data-ttu-id="61870-173">Om du vill visa information om en fabrik klickar du på raden i fabrikslistan.</span><span class="sxs-lookup"><span data-stu-id="61870-173">To view the details of a factory, click the row in the factory list.</span></span>

4. <span data-ttu-id="61870-174">Om du vill visa information om en produktionslinje klickar du på raden i listan.</span><span class="sxs-lookup"><span data-stu-id="61870-174">To view the details of a production line, click the row in the list.</span></span>

5. <span data-ttu-id="61870-175">Om du vill visa publicerade OPC UA-noder för en station i produktionslinjen klickar du på raden i listan.</span><span class="sxs-lookup"><span data-stu-id="61870-175">To view the published OPC UA nodes of a station on the production line, click the row in the list.</span></span>

6. <span data-ttu-id="61870-176">Om du vill visa information om en viss nod i stationen klickar du på raden i listan.</span><span class="sxs-lookup"><span data-stu-id="61870-176">To view details on a specific node in the station, click the row in the list.</span></span> <span data-ttu-id="61870-177">Den här åtgärden öppnar en kontextpanel med Time Series Insights-visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="61870-177">This action launches the context panel with Time Series Insights visualizations.</span></span> <span data-ttu-id="61870-178">Klicka på dessa diagram om du vill göra vidare analyser i Time Series Insights-utforskarmiljön.</span><span class="sxs-lookup"><span data-stu-id="61870-178">Click these graphs to do further analysis in the Time Series Insights explorer environment.</span></span>

## <a name="view-map"></a><span data-ttu-id="61870-179">Visa karta</span><span class="sxs-lookup"><span data-stu-id="61870-179">View map</span></span>

<span data-ttu-id="61870-180">Om din prenumeration ger åtkomst till Bing Maps-API:t visar *fabrikskartan* geografisk plats och status för alla fabriker i lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-180">If your subscription has access to the Bing Maps API, the *Factories* map shows you the geographical location and status of all the factories in the solution.</span></span> <span data-ttu-id="61870-181">Klicka på platserna som visas på kartan om du vill visa mer detaljerad information om platsen.</span><span class="sxs-lookup"><span data-stu-id="61870-181">To drill into the location details, click the locations displayed on the map.</span></span>

![Karta för den förkonfigurerade lösningen Ansluten fabrik][cf-img-map]

## <a name="view-alerts"></a><span data-ttu-id="61870-183">Visa aviseringar</span><span class="sxs-lookup"><span data-stu-id="61870-183">View alerts</span></span>

<span data-ttu-id="61870-184">**Aviseringspanelen** visar aviseringar som genererats på grund av ett rapporterat värde eller ett beräknat OEE/KPI-värde som överstiger dess konfigurerade tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="61870-184">The **Alert** panel shows you alerts generated due to a reported value or a calculated OEE/KPI value exceeding its configured threshold.</span></span> <span data-ttu-id="61870-185">Den här panelen visar aviseringar på varje nivå i hierarkin, från vyn på stationsnivå till vyn på global nivå.</span><span class="sxs-lookup"><span data-stu-id="61870-185">This panel displays alerts at each level of the hierarchy, from the station level view to the global view.</span></span> <span data-ttu-id="61870-186">Aviseringarna innehåller en beskrivningen av aviseringen, datum, tid, plats och antal förekomster.</span><span class="sxs-lookup"><span data-stu-id="61870-186">The alerts contain a description of the alert, date, time, location, and number of occurrences.</span></span> <span data-ttu-id="61870-187">Du kan få information om vad som orsakat aviseringen med hjälp av Time Series Insights-data.</span><span class="sxs-lookup"><span data-stu-id="61870-187">You can gain insights in to the data that caused the alert using the Time Series Insights data.</span></span> <span data-ttu-id="61870-188">Time Series Insights-data visualiseras i aviseringar (om tillämpligt).</span><span class="sxs-lookup"><span data-stu-id="61870-188">The Time Series Insights data is visualized in the alerts where applicable.</span></span> <span data-ttu-id="61870-189">Om du är administratör kan vidta du standardåtgärder för aviseringar, till exempel:</span><span class="sxs-lookup"><span data-stu-id="61870-189">If you are an Administrator, you can take default actions on the alerts such as:</span></span>

* <span data-ttu-id="61870-190">Stänga aviseringen.</span><span class="sxs-lookup"><span data-stu-id="61870-190">Close the alert.</span></span>
* <span data-ttu-id="61870-191">Bekräfta aviseringen.</span><span class="sxs-lookup"><span data-stu-id="61870-191">Acknowledge the alert.</span></span>

<span data-ttu-id="61870-192">Du kan också kan du utföra mer komplexa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="61870-192">Optionally, you can take more complex actions.</span></span> <span data-ttu-id="61870-193">För Pressure OPC UA-noden i sammansättningen kan du till exempel:</span><span class="sxs-lookup"><span data-stu-id="61870-193">For example, for the Pressure OPC UA Node of the Assembly you could:</span></span>

* <span data-ttu-id="61870-194">Visa mer information på en webbsida i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="61870-194">Show supporting information in a web page in a new browser window.</span></span>
* <span data-ttu-id="61870-195">Förebygg orsaken till aviseringen genom att anropa en OPC UA-metod på enheten.</span><span class="sxs-lookup"><span data-stu-id="61870-195">Mitigate the cause of the alert by calling an OPC UA method on the device.</span></span>
* <span data-ttu-id="61870-196">Välja att standardåtgärderna inte ska vara tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="61870-196">Suppress the availability of the default actions.</span></span>

    ![Aviseringar för den förkonfigurerade lösningen Ansluten fabrik][cf-img-alerts]

> [!NOTE]
> <span data-ttu-id="61870-198">De här aviseringarna genereras av regler som har angetts i en konfigurationsfil i den förinställda lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-198">These alerts are generated by rules that are specified in a configuration file in the preconfigured solution.</span></span> <span data-ttu-id="61870-199">Dessa regler kan generera aviseringar när OEE- eller KPI-värdena eller OPC UA-nodvärden överskrider de angivna tröskelvärdena.</span><span class="sxs-lookup"><span data-stu-id="61870-199">These rules can generate alerts when the OEE or KPI figures or OPC UA Node values are exceeding their configured threshold.</span></span>

1. <span data-ttu-id="61870-200">**Aviseringspanelen** visar de aviseringar som genererats i den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-200">The **Alerts panel** shows the alerts generated in this solution.</span></span>

2. <span data-ttu-id="61870-201">Klicka på en avisering på aviseringspanelen om du vill se mer information om aviseringen.</span><span class="sxs-lookup"><span data-stu-id="61870-201">To view the details of an alert, click the alert in the alerts panel.</span></span>

3. <span data-ttu-id="61870-202">Du kan analysera aviseringsinformation mer i detalj genom att klicka på diagrammet på aviseringspanelen för att öppna Time Series Insights-utforskarmiljön.</span><span class="sxs-lookup"><span data-stu-id="61870-202">To further analyze the alert data, click the graph in the alert panel to open the Time Series Insights explorer environment.</span></span>

4. <span data-ttu-id="61870-203">På aviseringspanelen finns flera åtgärder du kan vidta för aviseringen.</span><span class="sxs-lookup"><span data-stu-id="61870-203">To address the alert, several actions are available in the alert panel.</span></span> <span data-ttu-id="61870-204">Välj ett lämpligt alternativ och klicka på knappen för att utföra åtgärden.</span><span class="sxs-lookup"><span data-stu-id="61870-204">Choose the appropriate option for you and click the execute action command button.</span></span>

## <a name="view-overall-equipment-efficiency"></a><span data-ttu-id="61870-205">Visa OEE (Overall Equipment Efficiency)</span><span class="sxs-lookup"><span data-stu-id="61870-205">View overall equipment efficiency</span></span>

<span data-ttu-id="61870-206">OEE är nyckeltal för att mäta produktionseffektivitet.</span><span class="sxs-lookup"><span data-stu-id="61870-206">OEE rates the efficiency of the manufacturing process using a key production-related operational parameters.</span></span> <span data-ttu-id="61870-207">OEE är ett branschstandardmått som beräknas genom att multiplicera tillgänglighet, anläggningsutnyttjande och kvalitetsutbyte: OEE = tillgänglighet x anläggningsutnyttjande x kvalitetsutbyte.</span><span class="sxs-lookup"><span data-stu-id="61870-207">OEE is an industry standard measure calculated by multiplying the availability rate, performance rate, and quality rate: OEE = availability x performance x quality.</span></span>

![OEE för den förkonfigurerade lösningen Ansluten fabrik][cf-img-oee]

1. <span data-ttu-id="61870-209">Om du vill visa OEE för någon nivå i hierarkin navigerar du till den vy som du vill visa.</span><span class="sxs-lookup"><span data-stu-id="61870-209">To view OEE for any level in the hierarchy, navigate to the specific view you require.</span></span> <span data-ttu-id="61870-210">OEE för vyn visas på panelen tillsammans med de delar som utgör OEE-procentvärdet.</span><span class="sxs-lookup"><span data-stu-id="61870-210">The OEE for that view displays in the panel along with each of the elements that make up the OEE percentage.</span></span>

2. <span data-ttu-id="61870-211">Om du vill analysera OEE för en nivå i hierarkin i mer detalj klickar du på procentvärdet för OEE, tillgänglighet, prestanda eller kvalitet.</span><span class="sxs-lookup"><span data-stu-id="61870-211">To further analyze the OEE for any level in the hierarchy data, click either the OEE, availability, performance, or quality percentage.</span></span> <span data-ttu-id="61870-212">En kontextpanel visas med Time Series Insights-visualiseringar som visar data från den senaste timmen, de senaste 24 timmarna och de senaste sju dagarna.</span><span class="sxs-lookup"><span data-stu-id="61870-212">A context panel appears with Time Series Insights powered visualizations that shows data from the last hour, last 24 hours, and last 7 days.</span></span>

    ![TSI-visualisering för den förkonfigurerade lösningen Ansluten fabrik][cf-img-tsi-visualization]

3. <span data-ttu-id="61870-214">Om du vill analysera aviseringsdata mer i detalj klickar du på diagrammet på aviseringspanelen.</span><span class="sxs-lookup"><span data-stu-id="61870-214">To further analyze the alert data, click the graph in the alert panel.</span></span> <span data-ttu-id="61870-215">Den här åtgärden öppnar Time Series Insights-utforskarmiljön.</span><span class="sxs-lookup"><span data-stu-id="61870-215">This action opens the Time Series Insights explorer environment.</span></span>

    ![TSI-utforskaren för den förkonfigurerade lösningen Ansluten fabrik][cf-img-tsi-explorer]

## <a name="view-key-performance-indicators"></a><span data-ttu-id="61870-217">Visa KPI: er</span><span class="sxs-lookup"><span data-stu-id="61870-217">View Key Performance Indicators</span></span>

<span data-ttu-id="61870-218">Lösningen innehåller två KPI:er: *units per hour* (enheter per timme) och *energy used in kWh* (energi som används i kWh).</span><span class="sxs-lookup"><span data-stu-id="61870-218">The solution provides two key performance indicators, *units per hour* and *energy used in kWh*.</span></span>

![KPI för den förkonfigurerade lösningen Ansluten fabrik][cf-img-kpi]

1. <span data-ttu-id="61870-220">Om du vill visa enheter per timme eller energi som används för någon nivå i hierarkin navigerar du till den vy som du vill visa.</span><span class="sxs-lookup"><span data-stu-id="61870-220">To view units per hour or energy used for any level in the hierarchy, navigate to the specific view you require.</span></span> <span data-ttu-id="61870-221">Enheter per timme och energi som används visas på panelen.</span><span class="sxs-lookup"><span data-stu-id="61870-221">The units per hour and energy used display in the panel.</span></span>

2. <span data-ttu-id="61870-222">Om du vill analysera enheter per timme eller förbrukad energi för en nivå i hierarkin i mer detalj klickar du på mätaren på panelen **KPI:er**.</span><span class="sxs-lookup"><span data-stu-id="61870-222">To analyze units per hour or energy used for any level in the hierarchy further, click the gauge in the **Key Performance Indicators** panel.</span></span> <span data-ttu-id="61870-223">En kontextpanel visas med Time Series Insights-visualiseringar som visar data från den senaste timmen, de senaste 24 timmarna och de senaste sju dagarna.</span><span class="sxs-lookup"><span data-stu-id="61870-223">A context panel appears with Time Series Insights powered visualizations enabling you to view data from the last hour, the last 24 hours, and last 7 days.</span></span>

## <a name="scenario-review"></a><span data-ttu-id="61870-224">Scenariogranskning</span><span class="sxs-lookup"><span data-stu-id="61870-224">Scenario review</span></span>

<span data-ttu-id="61870-225">I det här scenariot övervakade du OEE- och KPI-värden på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="61870-225">In this scenario, you monitored your factories OEE and KPIs values, in the dashboard.</span></span> <span data-ttu-id="61870-226">Sedan använde du Time Series Insights för att visa mer information och detaljerade telemetridata för OEE och KPI:er för att kunna identifiera avvikelser.</span><span class="sxs-lookup"><span data-stu-id="61870-226">You then used Time Series Insights to provide more information to help drill further into the telemetry data for OEE and KPIs to help with detecting anomalies.</span></span> <span data-ttu-id="61870-227">Du använde också aviseringspanelen för att visa problem med dina fabriker och du använde tillgängliga alternativ för att åtgärda orsaken till aviseringen.</span><span class="sxs-lookup"><span data-stu-id="61870-227">You also used the alert panel to view issues with your factories and you used the actions available to you to resolve the alert.</span></span>

## <a name="other-features"></a><span data-ttu-id="61870-228">Andra funktioner</span><span class="sxs-lookup"><span data-stu-id="61870-228">Other features</span></span>

<span data-ttu-id="61870-229">I följande avsnitt beskrivs några ytterligare funktioner i lösningen Ansluten fabrik som inte beskrivs i scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="61870-229">The following sections describe some additional features of the connected factory solution that are not described in the previous scenario.</span></span>

## <a name="apply-filters"></a><span data-ttu-id="61870-230">Använda filter</span><span class="sxs-lookup"><span data-stu-id="61870-230">Apply filters</span></span>

1. <span data-ttu-id="61870-231">Klicka på **sparren** för att visa en lista över tillgängliga filter på panelen med fabriksplatser eller på panelen med aviseringar.</span><span class="sxs-lookup"><span data-stu-id="61870-231">Click the **chevron** to display a list of available filters in either the factory locations panel or the alerts panel.</span></span>

2. <span data-ttu-id="61870-232">Filterpanelen visas.</span><span class="sxs-lookup"><span data-stu-id="61870-232">The filters panel is displayed for you.</span></span> 

    ![Filter för den förkonfigurerade lösningen Ansluten fabrik][cf-img-alert-filter]

3. <span data-ttu-id="61870-234">Välj önskat filter.</span><span class="sxs-lookup"><span data-stu-id="61870-234">Choose the filter that you require.</span></span> <span data-ttu-id="61870-235">Du kan också skriva fritext i filterfälten.</span><span class="sxs-lookup"><span data-stu-id="61870-235">It is also possible to type free text into the filter fields.</span></span>

4. <span data-ttu-id="61870-236">Sedan tillämpas filtret.</span><span class="sxs-lookup"><span data-stu-id="61870-236">The filter is then applied for you.</span></span> <span data-ttu-id="61870-237">Filterläget visas i instrumentpanelen med hjälp av en tratt som visas i tabellerna för fabriker och aviseringar.</span><span class="sxs-lookup"><span data-stu-id="61870-237">The filter state is also shown in the dashboard via a funnel that displays in the factories and alerts tables.</span></span>

    ![Filter för den förkonfigurerade lösningen Ansluten fabrik][cf-img-alert-filter-funnel]

    > [!NOTE]
    > <span data-ttu-id="61870-239">Ett aktivt filter påverkar inte de OEE och KPI-värden som visas. Det filtrerar bara listinnehållet.</span><span class="sxs-lookup"><span data-stu-id="61870-239">An active filter does not affect the displayed OEE and KPI values, it only filters the list contents.</span></span>

5. <span data-ttu-id="61870-240">Om du vill ta bort ett filter klickar du på tratten och på filtret på kontextpanelen för filtret.</span><span class="sxs-lookup"><span data-stu-id="61870-240">To clear a filter, click the funnel and click filter in the filter context panel.</span></span> <span data-ttu-id="61870-241">Texten **Alla** visas i tabellerna för fabriker och aviseringar.</span><span class="sxs-lookup"><span data-stu-id="61870-241">The text **All** is displayed in the factories and alerts tables.</span></span>

## <a name="browse-an-opc-ua-server"></a><span data-ttu-id="61870-242">Navigera i en OPC UA-serverläsare</span><span class="sxs-lookup"><span data-stu-id="61870-242">Browse an OPC UA server</span></span>

<span data-ttu-id="61870-243">När du distribuerar den förkonfigurerade lösningen etableras automatiskt simulerade OPC UA-servrar som du kan navigerar i via serverläsaren.</span><span class="sxs-lookup"><span data-stu-id="61870-243">When you deploy the preconfigured solution, you automatically provision simulated OPC UA servers that you can browse via the solution browser.</span></span> <span data-ttu-id="61870-244">Dessa servrar är *simulerade OPC UA-servrar*.</span><span class="sxs-lookup"><span data-stu-id="61870-244">These servers are *simulated OPC UA servers*.</span></span> <span data-ttu-id="61870-245">Med simulerade servrar kan du enkelt experimentera med den förkonfigurerade lösningen utan att du behöver distribuera verkliga, fysiska servrar.</span><span class="sxs-lookup"><span data-stu-id="61870-245">Simulated servers make it easy for you to experiment with the preconfigured solution without the need to deploy real, physical servers.</span></span> <span data-ttu-id="61870-246">Om du vill ansluta en verklig OPC UA-server till lösningen kan du läsa självstudiekursen [Connect your OPC UA device to the connected factory preconfigured solution][lnk-connect-cf] (Ansluta OPC UA-enheten till den förkonfigurerade lösningen Ansluten fabrik).</span><span class="sxs-lookup"><span data-stu-id="61870-246">If you do want to connect a real OPC UA server to the solution, see the [Connect your OPC UA device to the connected factory preconfigured solution][lnk-connect-cf] tutorial.</span></span>

1. <span data-ttu-id="61870-247">Klicka på **fabriksikonen** i navigeringsfältet i instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="61870-247">Click the **factory icon** in the dashboard navigation bar.</span></span>

    ![Serverläsare för den förkonfigurerade lösningen Ansluten fabrik][cf-img-server-browser]

2. <span data-ttu-id="61870-249">Välj en av servrarna i den förkonfigurerade listan.</span><span class="sxs-lookup"><span data-stu-id="61870-249">Choose one of the servers from the preconfigured list.</span></span> <span data-ttu-id="61870-250">Listan visar de servrar som är distribuerade åt dig i den förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-250">This list shows the servers that are deployed for you in the preconfigured solution.</span></span>

    ![Serverurval för den förkonfigurerade lösningen Ansluten fabrik][cf-img-server-choice]

3. <span data-ttu-id="61870-252">Klicka på **Connect** (Anslut) för att visa en dialogruta för säkerhet.</span><span class="sxs-lookup"><span data-stu-id="61870-252">Click **Connect**, a security dialog displays.</span></span> <span data-ttu-id="61870-253">För simuleringen är det säkert att klicka på **Proceed** (Fortsätt).</span><span class="sxs-lookup"><span data-stu-id="61870-253">For the simulation, it is safe to click **Proceed**</span></span>

4. <span data-ttu-id="61870-254">Om du vill expandera någon av noderna i serverträdet klickar du på noden.</span><span class="sxs-lookup"><span data-stu-id="61870-254">To expand any of the nodes in the server tree, click it.</span></span> <span data-ttu-id="61870-255">Noder som publicerar telemetri har ett skalstreck bredvid sig.</span><span class="sxs-lookup"><span data-stu-id="61870-255">Nodes that are publishing telemetry have a tick mark beside them.</span></span>

    ![Serverträd för den förkonfigurerade lösningen Ansluten fabrik][cf-img-server-tree]

5. <span data-ttu-id="61870-257">Högerklicka på ett objekt om du vill läsa, skriva, publicera eller anropa noden.</span><span class="sxs-lookup"><span data-stu-id="61870-257">Right-click an item to read, write, publish, or call that node.</span></span> <span data-ttu-id="61870-258">Vilka åtgärder som är tillgängliga beror på dina behörigheter och nodens attribut.</span><span class="sxs-lookup"><span data-stu-id="61870-258">The actions available to you depend on your permissions and the attributes of the node.</span></span> <span data-ttu-id="61870-259">Läsalternativet visar en kontextpanel med värdet för den specifika noden.</span><span class="sxs-lookup"><span data-stu-id="61870-259">The read option to displays a context panel showing the value of the specific node.</span></span> <span data-ttu-id="61870-260">Skrivalternativet visar en kontextpanel där du kan ange ett nytt värde.</span><span class="sxs-lookup"><span data-stu-id="61870-260">The write option displays a context panel where you can enter a new value.</span></span> <span data-ttu-id="61870-261">Anropsalternativet visar en nod där du kan ange parametrar för anropet.</span><span class="sxs-lookup"><span data-stu-id="61870-261">The call option displays a node where you can enter the parameters for the call.</span></span>

## <a name="publish-a-node"></a><span data-ttu-id="61870-262">Publicera en nod</span><span class="sxs-lookup"><span data-stu-id="61870-262">Publish a node</span></span>

<span data-ttu-id="61870-263">När du navigerar i en *simulerad OPC UA-server* kan du välja att publicera nya noder.</span><span class="sxs-lookup"><span data-stu-id="61870-263">When you browse a *simulated OPC UA server*, you can also choose to publish new nodes.</span></span> <span data-ttu-id="61870-264">Du kan analysera telemetri från dessa noder i lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-264">You can analyze the telemetry from these nodes in the solution.</span></span> <span data-ttu-id="61870-265">Med dessa *simulerade OPC UA-servrar* kan du enkelt experimentera med den förkonfigurerade lösningen utan att distribuera verkliga fysiska enheter.</span><span class="sxs-lookup"><span data-stu-id="61870-265">These *simulated OPC UA servers* make it easy to experiment with the preconfigured solution without deploying real, physical devices.</span></span>

1. <span data-ttu-id="61870-266">Bläddra till en nod i OPC UA-serverläsarträdet som du vill publicera.</span><span class="sxs-lookup"><span data-stu-id="61870-266">Browse to a node in the OPC UA server browser tree that you wish to publish.</span></span>

2. <span data-ttu-id="61870-267">Högerklicka på noden.</span><span class="sxs-lookup"><span data-stu-id="61870-267">Right-click the node.</span></span>

3. <span data-ttu-id="61870-268">Välj **Publish** (Publicera).</span><span class="sxs-lookup"><span data-stu-id="61870-268">Choose **Publish**.</span></span>

    ![Ansluten fabrik publicerar nod][cf-img-publish-node]

4. <span data-ttu-id="61870-270">En kontextpanel visas som anger att publiceringen är klar.</span><span class="sxs-lookup"><span data-stu-id="61870-270">A context panel appears which tells you that the publish has succeeded.</span></span> <span data-ttu-id="61870-271">Noden visas i vyn på stationsnivå med en bock bredvid den.</span><span class="sxs-lookup"><span data-stu-id="61870-271">The node appears in the station level view with a check mark beside it.</span></span>

    ![Slutförd publicering i den förkonfigurerade lösningen Ansluten fabrik][cf-img-publish-success]

## <a name="command-and-control"></a><span data-ttu-id="61870-273">Kommando och kontroll</span><span class="sxs-lookup"><span data-stu-id="61870-273">Command and control</span></span>

<span data-ttu-id="61870-274">Med den anslutna fabriken kan du styra dina industriella enheter direkt från molnet.</span><span class="sxs-lookup"><span data-stu-id="61870-274">The connected factory allows you command and control your industry devices directly from the cloud.</span></span> <span data-ttu-id="61870-275">Du kan använda funktionen till att vidta åtgärder vid aviseringar som genereras av enheten.</span><span class="sxs-lookup"><span data-stu-id="61870-275">You can use this feature to respond to alerts generated by the device.</span></span> <span data-ttu-id="61870-276">Du kan till exempel skicka ett kommando till enheten från molnet.</span><span class="sxs-lookup"><span data-stu-id="61870-276">For example, you could send a command to the device from the cloud.</span></span> <span data-ttu-id="61870-277">Tillgängliga kommandon finns i noden **StationCommands** i OPC UA-serverläsarträdet.</span><span class="sxs-lookup"><span data-stu-id="61870-277">You can find the available commands in the **StationCommands** node in the OPC UA servers browser tree.</span></span> <span data-ttu-id="61870-278">I det här scenariot öppnar du en övertrycksventil på en monteringsstation i en produktionslinje i München.</span><span class="sxs-lookup"><span data-stu-id="61870-278">In this scenario, you open a pressure release valve on the assembly station of a production line in Munich.</span></span> <span data-ttu-id="61870-279">För att kunna styra funktioner måste du ha rollen **Administratör** för distributionen av den förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-279">To use the command and control functionality, you must be in the **Administrator** role for the preconfigured solution deployment.</span></span>

1. <span data-ttu-id="61870-280">Bläddra till noden **StationCommands** i OPC UA-serverläsarträdet.</span><span class="sxs-lookup"><span data-stu-id="61870-280">Browse to the **StationCommands** node in the OPC UA server browser tree.</span></span>

2. <span data-ttu-id="61870-281">Välj det kommando som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="61870-281">Choose the command that you wish use.</span></span>

3. <span data-ttu-id="61870-282">Högerklicka på noden.</span><span class="sxs-lookup"><span data-stu-id="61870-282">Right-click the node.</span></span>

4. <span data-ttu-id="61870-283">Välj **Call** (Anropa).</span><span class="sxs-lookup"><span data-stu-id="61870-283">Choose **Call**.</span></span>

    ![Anropskommando för den förkonfigurerade lösningen Ansluten fabrik][cf-img-call-command]

5. <span data-ttu-id="61870-285">En kontextpanel visas som anger vilken metod du anropar och eventuell parameterinformation.</span><span class="sxs-lookup"><span data-stu-id="61870-285">A context panel appears informing you which method you are about to call and any parameter details is applicable.</span></span>

6. <span data-ttu-id="61870-286">Välj **Call** (Anropa).</span><span class="sxs-lookup"><span data-stu-id="61870-286">Choose **Call**.</span></span>

    ![Kontextpanel för anrop för den förkonfigurerade lösningen Ansluten fabrik][cf-img-call-context]

7. <span data-ttu-id="61870-288">Kontextpanelen uppdateras för att informera dig att metodanropet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="61870-288">The context panel is updated to inform you that the method call succeeded.</span></span> <span data-ttu-id="61870-289">Du kan verifiera att anropet har slutförts genom att läsa av värdet för trycknoden som uppdaterades av anropet.</span><span class="sxs-lookup"><span data-stu-id="61870-289">You can verify the call succeeded by reading the value of the pressure node that updated as a result of the call.</span></span>

    ![Slutfört anrop för den förkonfigurerade lösningen Ansluten fabrik][cf-img-call-success]


## <a name="behind-the-scenes"></a><span data-ttu-id="61870-291">I bakgrunden</span><span class="sxs-lookup"><span data-stu-id="61870-291">Behind the scenes</span></span>

<span data-ttu-id="61870-292">När du distribuerar en förkonfigurerad lösning skapar distributionsprocessen flera resurser i Azure-prenumerationen som du valt.</span><span class="sxs-lookup"><span data-stu-id="61870-292">When you deploy a preconfigured solution, the deployment process creates multiple resources in the Azure subscription you selected.</span></span> <span data-ttu-id="61870-293">Du kan visa dessa resurser på Azure-[portalen][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="61870-293">You can view these resources in the Azure [portal][lnk-portal].</span></span> <span data-ttu-id="61870-294">Under distributionsprocessen skapas en **resursgrupp** med ett namn baserat på det namn som du valde för den förkonfigurerade lösningen:</span><span class="sxs-lookup"><span data-stu-id="61870-294">The deployment process creates a **resource group** with a name based on the name you choose for your preconfigured solution:</span></span>

![Förkonfigurerad lösning på Azure-portalen][img-cf-portal]

<span data-ttu-id="61870-296">Du kan visa inställningarna för varje resurs genom att välja resursen i listan över resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="61870-296">You can view the settings of each resource by selecting it in the list of resources in the resource group.</span></span>

<span data-ttu-id="61870-297">Du kan också visa källkoden för den förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-297">You can also view the source code for the preconfigured solution.</span></span> <span data-ttu-id="61870-298">Källkoden för den förkonfigurerade lösningen Ansluten fabrik finns i [azure-iot-connected-factory][lnk-cfgithub]-databasen på GitHub:</span><span class="sxs-lookup"><span data-stu-id="61870-298">The connected factory preconfigured solution source code is in the [azure-iot-connected-factory][lnk-cfgithub] GitHub repository:</span></span>

<span data-ttu-id="61870-299">När du är klar kan du ta bort den förkonfigurerade lösningen från Azure-prenumerationen på webbplatsen [azureiotsuite.com][lnk-azureiotsuite].</span><span class="sxs-lookup"><span data-stu-id="61870-299">When you are done, you can delete the preconfigured solution from your Azure subscription on the [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="61870-300">På den här webbplatsen kan du enkelt ta bort alla resurser som etablerades när du skapade den förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="61870-300">This site enables you to easily delete all the resources that were provisioned when you created the preconfigured solution.</span></span>

> [!NOTE]
> <span data-ttu-id="61870-301">För att vara säker på att du tar bort allt som hör till den förkonfigurerade lösningen tar du bort den från [azureiotsuite.com][lnk-azureiotsuite].</span><span class="sxs-lookup"><span data-stu-id="61870-301">To ensure that you delete everything related to the preconfigured solution, delete it on the [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="61870-302">Ta inte bort resursgruppen i portalen.</span><span class="sxs-lookup"><span data-stu-id="61870-302">Do not delete the resource group in the portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="61870-303">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="61870-303">Next Steps</span></span>

<span data-ttu-id="61870-304">Nu när du har distribuerat en fungerande förkonfigurerad lösning kan du fortsätta och lära dig mer om IoT Suite genom att läsa följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="61870-304">Now that you’ve deployed a working preconfigured solution, you can continue getting started with IoT Suite by reading the following articles:</span></span>

* <span data-ttu-id="61870-305">[Genomgång av den förkonfigurerade lösningen Ansluten fabrik][lnk-rm-walkthrough]</span><span class="sxs-lookup"><span data-stu-id="61870-305">[Connected factory preconfigured solution walkthrough][lnk-rm-walkthrough]</span></span>
* <span data-ttu-id="61870-306">[Ansluta enheten till den förkonfigurerade lösningen Ansluten fabrik][lnk-connect-cf]</span><span class="sxs-lookup"><span data-stu-id="61870-306">[Connect your device to the Connected factory preconfigured solution][lnk-connect-cf]</span></span>
* <span data-ttu-id="61870-307">[Behörigheter på webbplatsen azureiotsuite.com][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="61870-307">[Permissions on the azureiotsuite.com site][lnk-permissions]</span></span>

[img-cf-home]:media/iot-suite-connected-factory-overview/cf-dashboard.png
[img-launch-solution]: media/iot-suite-connected-factory-overview/launch-cf.png
[cf-img-menu]: media/iot-suite-connected-factory-overview/cf-dashboard-menu.png
[cf-img-factories]:media/iot-suite-connected-factory-overview/cf-dashboard-factory.png
[cf-img-map]:media/iot-suite-connected-factory-overview/cf-dashboard-map.png
[cf-img-alerts]:media/iot-suite-connected-factory-overview/cf-dashboard-alerts.png
[cf-img-oee]:media/iot-suite-connected-factory-overview/cf-dashboard-oee.png
[cf-img-kpi]:media/iot-suite-connected-factory-overview/cf-dashboard-kpi.png
[cf-img-tsi-visualization]:media/iot-suite-connected-factory-overview/cf-dashboard-tsi.png
[cf-img-tsi-explorer]:media/iot-suite-connected-factory-overview/tsi-explorer.png
[cf-img-server-browser]: media/iot-suite-connected-factory-overview/cf-dashboard-browser.png
[cf-img-server-choice]: media/iot-suite-connected-factory-overview/cf-select-server.png
[cf-img-server-tree]:media/iot-suite-connected-factory-overview/cf-server-tree.png
[cf-img-publish-node]:media/iot-suite-connected-factory-overview/cf-publish-node1.png
[cf-img-publish-success]:media/iot-suite-connected-factory-overview/cf-publish-success.png
[cf-img-call-command]:media/iot-suite-connected-factory-overview/cf-command-and-control-call.png
[cf-img-call-context]:media/iot-suite-connected-factory-overview/cf-command-and-control-call-button.png
[cf-img-call-success]:media/iot-suite-connected-factory-overview/cf-command-and-control-succeed.png
[img-cf-portal]:media/iot-suite-connected-factory-overview/cf-resource-group.png
[cf-img-alert-filter]:media/iot-suite-connected-factory-overview/cf-filter.png
[cf-img-alert-filter-funnel]:media/iot-suite-connected-factory-overview/cf-filter-funnel.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-portal]: http://portal.azure.com/
[lnk-cfgithub]: https://github.com/Azure/azure-iot-connected-factory
[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md