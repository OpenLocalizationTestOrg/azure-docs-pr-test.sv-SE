---
title: "aaaAzure IoT Suite anslutna factory översikt | Microsoft Docs"
description: "En beskrivning av hello Azure IoT Suite ansluten factory förkonfigurerade lösningen."
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
ms.openlocfilehash: 929b5ed41ef7d82c9b4480d02aa3f0db38931919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-connected-factory-preconfigured-solution"></a><span data-ttu-id="73758-103">Kom igång med hello anslutna factory förkonfigurerade lösningen</span><span class="sxs-lookup"><span data-stu-id="73758-103">Get started with hello connected factory preconfigured solution</span></span>

<span data-ttu-id="73758-104">Azure IoT Suite [förkonfigurerade lösningar] [ lnk-preconfigured-solutions] kombinera flera Azure IoT services toodeliver slutpunkt till slutpunkt-lösningar som implementerar vanliga scenarier för IoT-företag.</span><span class="sxs-lookup"><span data-stu-id="73758-104">Azure IoT Suite [preconfigured solutions][lnk-preconfigured-solutions] combine multiple Azure IoT services toodeliver end-to-end solutions that implement common IoT business scenarios.</span></span> <span data-ttu-id="73758-105">Hej *anslutna factory* förkonfigurerade lösningen ansluter tooand Övervakare industriella enheter.</span><span class="sxs-lookup"><span data-stu-id="73758-105">hello *connected factory* preconfigured solution connects tooand monitors your industrial devices.</span></span> <span data-ttu-id="73758-106">Du kan använda hello lösning tooanalyze hello dataström från dina enheter och toodrive operativa produktivitet och lönsamhet.</span><span class="sxs-lookup"><span data-stu-id="73758-106">You can use hello solution tooanalyze hello stream of data from your devices and toodrive operational productivity and profitability.</span></span>

<span data-ttu-id="73758-107">Den här kursen visar hur tooprovision hello anslutna factory förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="73758-107">This tutorial shows you how tooprovision hello connected factory preconfigured solution.</span></span> <span data-ttu-id="73758-108">Även vägleder dig genom hello grundläggande funktioner i hello förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="73758-108">It also walks you through hello basic features of hello preconfigured solution.</span></span> <span data-ttu-id="73758-109">Du kan komma åt många av dessa funktioner från hello lösning *instrumentpanelen* som distribuerar som en del av hello förkonfigurerade lösningen:</span><span class="sxs-lookup"><span data-stu-id="73758-109">You can access many of these features from hello solution *dashboard* that deploys as part of hello preconfigured solution:</span></span>

![instrumentpanel för den förkonfigurerade lösningen ansluten fabrik][img-cf-home]

<span data-ttu-id="73758-111">toocomplete den här självstudiekursen kommer du behöver en aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="73758-111">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="73758-112">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="73758-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="73758-113">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="73758-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
> 
> 

## <a name="provision-hello-solution"></a><span data-ttu-id="73758-114">Etablera hello lösning</span><span class="sxs-lookup"><span data-stu-id="73758-114">Provision hello solution</span></span>

1. <span data-ttu-id="73758-115">Logga in med dina inloggningsuppgifter för Azure-konto tooazureiotsuite.com och klicka på ”**+**” toocreate en lösning.</span><span class="sxs-lookup"><span data-stu-id="73758-115">Log on tooazureiotsuite.com using your Azure account credentials, and click "**+**" toocreate a solution.</span></span>
2. <span data-ttu-id="73758-116">Klicka på **Välj** på hello **anslutna factory** panelen.</span><span class="sxs-lookup"><span data-stu-id="73758-116">Click **Select** on hello **Connected factory** tile.</span></span>
3. <span data-ttu-id="73758-117">Ange ett **lösningsnamn** för den förkonfigurerade anslutna fabriken för fjärrövervakning.</span><span class="sxs-lookup"><span data-stu-id="73758-117">Enter a **Solution name** for your connected factory preconfigured solution.</span></span>
4. <span data-ttu-id="73758-118">Välj hello **prenumeration** och **Region** du vill toouse tooprovision hello-lösningen.</span><span class="sxs-lookup"><span data-stu-id="73758-118">Select hello **Subscription** and **Region** you want toouse tooprovision hello solution.</span></span>
5. <span data-ttu-id="73758-119">Klicka på **skapa lösningen** toobegin hello etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="73758-119">Click **Create Solution** toobegin hello provisioning process.</span></span> <span data-ttu-id="73758-120">Denna process brukar ta flera minuter toorun.</span><span class="sxs-lookup"><span data-stu-id="73758-120">This process typically takes several minutes toorun.</span></span>

### <a name="while-you-wait-for-hello-provisioning-process-toocomplete"></a><span data-ttu-id="73758-121">Medan du väntar hello etablering processen toocomplete</span><span class="sxs-lookup"><span data-stu-id="73758-121">While you wait for hello provisioning process toocomplete</span></span>

1. <span data-ttu-id="73758-122">Klicka på hello panelen för din lösning med **etablering** status.</span><span class="sxs-lookup"><span data-stu-id="73758-122">Click hello tile for your solution with **Provisioning** status.</span></span>
2. <span data-ttu-id="73758-123">Meddelande hello **etablering tillstånd** som Azure-tjänster har distribuerats i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="73758-123">Notice hello **Provisioning states** as Azure services are deployed in your Azure subscription.</span></span>
3. <span data-ttu-id="73758-124">När etableringen är klar hello status ändras för**klar**.</span><span class="sxs-lookup"><span data-stu-id="73758-124">Once provisioning completes, hello status changes too**Ready**.</span></span>
4. <span data-ttu-id="73758-125">Klicka på panelen hello toosee hello information i lösningen i hello högra rutan.</span><span class="sxs-lookup"><span data-stu-id="73758-125">Click hello tile toosee hello details of your solution in hello right-hand pane.</span></span>

> [!NOTE]
> <span data-ttu-id="73758-126">Om du får problem som distribuerar hello förkonfigurerade lösningen granska [behörigheter på hello azureiotsuite.com platsen] [ lnk-permissions] och hello [anslutna factory vanliga frågor och svar](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="73758-126">If you encounter issues deploying hello preconfigured solution, review [Permissions on hello azureiotsuite.com site][lnk-permissions] and hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span> <span data-ttu-id="73758-127">Om hello problem kvarstår, skapa en tjänstbiljett på hello [portal][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="73758-127">If hello issues persist, create a service ticket on hello [portal][lnk-portal].</span></span>

<span data-ttu-id="73758-128">Finns det information som du förväntar dig toosee som inte visas för lösningen?</span><span class="sxs-lookup"><span data-stu-id="73758-128">Are there details you'd expect toosee that aren't listed for your solution?</span></span> <span data-ttu-id="73758-129">Lämna förslag på funktioner i [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span><span class="sxs-lookup"><span data-stu-id="73758-129">Give us feature suggestions on [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="73758-130">Scenarioöversikt</span><span class="sxs-lookup"><span data-stu-id="73758-130">Scenario overview</span></span>

<span data-ttu-id="73758-131">När du distribuerar hello anslutna factory förkonfigurerade lösningen, förinställd med resurser som gör att du toostep via ett vanligt scenario för industriella.</span><span class="sxs-lookup"><span data-stu-id="73758-131">When you deploy hello connected factory preconfigured solution, it is prepopulated with resources that enable you toostep through a common industrial scenario.</span></span> <span data-ttu-id="73758-132">I det här scenariot flera fabriker anslutna toohello lösning rapporten hello värden krävs toocompute övergripande utrustning effektivitet (OEE) och nyckeltal (KPI: er).</span><span class="sxs-lookup"><span data-stu-id="73758-132">In this scenario, several factories connected toohello solution report hello data values required toocompute overall equipment efficiency (OEE) and key performance indicators (KPIs).</span></span> <span data-ttu-id="73758-133">hello följande avsnitt visar hur till:</span><span class="sxs-lookup"><span data-stu-id="73758-133">hello following sections show you how to:</span></span>

* <span data-ttu-id="73758-134">Övervaka fabrik, produktionslinjer, station OEE och KPI-värden</span><span class="sxs-lookup"><span data-stu-id="73758-134">Monitor factory, production lines, station OEE, and KPI values</span></span>
* <span data-ttu-id="73758-135">Analysera hello telemetridata som genereras från dessa enheter med hjälp av Azure tid serien Insights</span><span class="sxs-lookup"><span data-stu-id="73758-135">Analyze hello telemetry data generated from these devices using Azure Time Series Insights</span></span>
* <span data-ttu-id="73758-136">Arbeta med aviseringar toofix problem</span><span class="sxs-lookup"><span data-stu-id="73758-136">Act on alerts toofix issues</span></span>

<span data-ttu-id="73758-137">En nyckelfunktion i det här scenariot är att du kan utföra dessa åtgärder via fjärranslutning från hello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="73758-137">A key feature of this scenario is that you can perform all these actions remotely from hello solution dashboard.</span></span> <span data-ttu-id="73758-138">Du behöver inte fysisk åtkomst toohello enheter.</span><span class="sxs-lookup"><span data-stu-id="73758-138">You do not need physical access toohello devices.</span></span>

## <a name="view-hello-solution-dashboard"></a><span data-ttu-id="73758-139">Visa hello lösning instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="73758-139">View hello solution dashboard</span></span>

<span data-ttu-id="73758-140">hello lösning instrumentpanelen kan du toomanage hello distribueras lösning.</span><span class="sxs-lookup"><span data-stu-id="73758-140">hello solution dashboard enables you toomanage hello deployed solution.</span></span> <span data-ttu-id="73758-141">Det här är en hierarkisk representation av en global fabrikskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="73758-141">It is a hierarchical representation of a global factory configuration.</span></span> <span data-ttu-id="73758-142">Du kan till exempel se OEE och KPI:er, publicera nya noder för telemetri och åtgärdsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="73758-142">For example, you can view OEE and KPIs, publish new nodes for telemetry and action alerts.</span></span>

1. <span data-ttu-id="73758-143">När hello etableringen har slutförts och hello panelen för din förkonfigurerade lösning anger **klar**, Välj **starta** tooopen anslutna factory lösning portalen på en ny flik.</span><span class="sxs-lookup"><span data-stu-id="73758-143">When hello provisioning is complete and hello tile for your preconfigured solution indicates **Ready**, choose **Launch** tooopen your connected factory solution portal in a new tab.</span></span>

    ![Starta hello förkonfigurerade lösningen][img-launch-solution]

1. <span data-ttu-id="73758-145">Som standard visar hello lösning portal hello *instrumentpanelen*.</span><span class="sxs-lookup"><span data-stu-id="73758-145">By default, hello solution portal shows hello *dashboard*.</span></span> <span data-ttu-id="73758-146">toonavigate tooother områden av hello portal använder hello-menyn i hello vänster sida av hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="73758-146">toonavigate tooother areas of hello portal, use hello menu on hello left-hand side of hello page.</span></span>

    ![Instrumentpanel för den förkonfigurerade lösningen Ansluten fabrik][cf-img-menu]

<span data-ttu-id="73758-148">hello instrumentpanelen visar hello följande information:</span><span class="sxs-lookup"><span data-stu-id="73758-148">hello dashboard displays hello following information:</span></span>

* <span data-ttu-id="73758-149">En **Factory listan** panelen som visar hello status, plats och den aktuella produktion konfigurationen i hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="73758-149">A **Factory list** panel that shows hello status, location, and current production configuration in hello solution.</span></span> <span data-ttu-id="73758-150">När du kör först hello lösning, finns det ett antal simulerade enheter.</span><span class="sxs-lookup"><span data-stu-id="73758-150">When you first run hello solution, there are a number of simulated devices.</span></span> <span data-ttu-id="73758-151">hello produktionen simuleringen består av tre verkliga OPC UA servrar per rad produktion som utför simulerade aktiviteter och dela data.</span><span class="sxs-lookup"><span data-stu-id="73758-151">hello production line simulation is composed of three real OPC UA servers per production line that perform simulated tasks and share data.</span></span> <span data-ttu-id="73758-152">Mer information om OPC UA finns hello [anslutna factory vanliga frågor och svar](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="73758-152">For more information about OPC UA, see hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>
* <span data-ttu-id="73758-153">En **kartan** att visar hello platsen för varje enhet ansluten toohello lösning.</span><span class="sxs-lookup"><span data-stu-id="73758-153">A **map** that displays hello location of each device connected toohello solution.</span></span> <span data-ttu-id="73758-154">hello-lösning kan använda hello Bing Maps API tooplot information på hello karta.</span><span class="sxs-lookup"><span data-stu-id="73758-154">hello solution can use hello Bing Maps API tooplot information on hello map.</span></span> <span data-ttu-id="73758-155">Om Bing Maps Enterprise API är aktiverat för din prenumeration används den här funktionen automatiskt.</span><span class="sxs-lookup"><span data-stu-id="73758-155">If your subscription is enabled for Bing Maps Enterprise API, then this feature is used automatically.</span></span> <span data-ttu-id="73758-156">Om inte, finns hello [vanliga frågor och svar] [ lnk-faq] toolearn hur toomake hello kartan dynamiska.</span><span class="sxs-lookup"><span data-stu-id="73758-156">If not, see hello [FAQ][lnk-faq] toolearn how toomake hello map dynamic.</span></span>
* <span data-ttu-id="73758-157">En panel för **aviseringar** som visar aviseringar som genereras när ett telemetri- eller OEE/KPI-värde överskrider ett visst tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="73758-157">An **Alerts** panel that displays alerts generated when a telemetry or OEE/KPI value exceeds a specific threshold.</span></span>
* <span data-ttu-id="73758-158">En **utrustning effektiviteten** panelen som visar hello OEE värden för hello hela företaget eller hello factory/produktion rad/station du visar.</span><span class="sxs-lookup"><span data-stu-id="73758-158">An **Overall Equipment Efficiency** panel that shows hello OEE values for hello whole enterprise, or hello factory/production line/station you are viewing.</span></span> <span data-ttu-id="73758-159">Det här värdet sammanställs från hello station visa toohello företagsnivå.</span><span class="sxs-lookup"><span data-stu-id="73758-159">This value is aggregated from hello station view toohello enterprise level.</span></span> <span data-ttu-id="73758-160">Hej OEE bild och dess ingående element kan analyseras ytterligare.</span><span class="sxs-lookup"><span data-stu-id="73758-160">hello OEE figure and its constituent elements can be further analyzed.</span></span>
* <span data-ttu-id="73758-161">**Prestationsindikatorer** panelen som visar hello antalet enheter som producerats och energi som används med hello hela företaget eller hello factory/produktion rad/station du visar.</span><span class="sxs-lookup"><span data-stu-id="73758-161">**Key Performance Indicators** panel that displays hello number of units produced and energy used by hello whole enterprise or hello factory/production line/station you are viewing.</span></span> <span data-ttu-id="73758-162">Dessa värden sammanställs från en station visa toohello enterprise-nivå.</span><span class="sxs-lookup"><span data-stu-id="73758-162">These values are aggregated from a station view toohello enterprise level.</span></span>

## <a name="view-factories"></a><span data-ttu-id="73758-163">Visa fabriker</span><span class="sxs-lookup"><span data-stu-id="73758-163">View factories</span></span>

<span data-ttu-id="73758-164">Hej *fabriker* panelen visar du hello geografiska placeringen för alla hello fabriker i hello lösning, status och aktuell konfiguration för produktion.</span><span class="sxs-lookup"><span data-stu-id="73758-164">hello *Factories* panel shows you hello geographical location of all hello factories in hello solution, their status, and current production configuration.</span></span> <span data-ttu-id="73758-165">Du kan navigera andra nivåer i hierarkin för hello lösning toohello hello platser listan.</span><span class="sxs-lookup"><span data-stu-id="73758-165">From hello locations list, you can navigate toohello other levels in hello solution hierarchy.</span></span> <span data-ttu-id="73758-166">hello rader i hello listan är hyperlänkar som länkar information om hello produktion rader på den platsen.</span><span class="sxs-lookup"><span data-stu-id="73758-166">hello rows in hello list are hyperlinks that link details of hello production lines at that location.</span></span> <span data-ttu-id="73758-167">Är det möjligt toodrill hello produktionen detaljer och ned toohello station nivån vyn.</span><span class="sxs-lookup"><span data-stu-id="73758-167">It is then possible toodrill into hello production line details and down toohello station level view.</span></span> <span data-ttu-id="73758-168">Du kan också använda en toohello filterlistan.</span><span class="sxs-lookup"><span data-stu-id="73758-168">You can also apply a filter toohello list.</span></span>

![Fabriker för den förkonfigurerade lösningen Ansluten fabrik][cf-img-factories] 

1. <span data-ttu-id="73758-170">Hej **Factory panelen** visar hello factory listan för den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="73758-170">hello **Factory panel** shows hello factory list for this solution.</span></span>

2. <span data-ttu-id="73758-171">hello factory i listan visas först sex fabriker som skapats av hello etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="73758-171">hello factory list initially shows six factories created by hello provisioning process.</span></span> <span data-ttu-id="73758-172">Du kan lägga till ytterligare simulerade och fysiska enheter toohello lösning.</span><span class="sxs-lookup"><span data-stu-id="73758-172">You can add additional simulated and physical devices toohello solution.</span></span>

3. <span data-ttu-id="73758-173">tooview hello detaljer för en fabrik Klicka hello rad i hello factory lista.</span><span class="sxs-lookup"><span data-stu-id="73758-173">tooview hello details of a factory, click hello row in hello factory list.</span></span>

4. <span data-ttu-id="73758-174">tooview hello information om en rad för produktion, klicka på hello rad i hello lista.</span><span class="sxs-lookup"><span data-stu-id="73758-174">tooview hello details of a production line, click hello row in hello list.</span></span>

5. <span data-ttu-id="73758-175">tooview hello publicerade OPC UA noder i en station hello produktion rad, klicka på hello rad i hello lista.</span><span class="sxs-lookup"><span data-stu-id="73758-175">tooview hello published OPC UA nodes of a station on hello production line, click hello row in hello list.</span></span>

6. <span data-ttu-id="73758-176">tooview information på en viss nod i hello station, klicka på hello rad i hello lista.</span><span class="sxs-lookup"><span data-stu-id="73758-176">tooview details on a specific node in hello station, click hello row in hello list.</span></span> <span data-ttu-id="73758-177">Den här åtgärden startar hello kontexten panel med tiden serien insikter visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="73758-177">This action launches hello context panel with Time Series Insights visualizations.</span></span> <span data-ttu-id="73758-178">Klicka på de här diagrammen toodo ytterligare analys i hello tid serien insikter explorer miljö.</span><span class="sxs-lookup"><span data-stu-id="73758-178">Click these graphs toodo further analysis in hello Time Series Insights explorer environment.</span></span>

## <a name="view-map"></a><span data-ttu-id="73758-179">Visa karta</span><span class="sxs-lookup"><span data-stu-id="73758-179">View map</span></span>

<span data-ttu-id="73758-180">Om din prenumeration har åtkomst toohello Bing Maps API, hello *fabriker* karta visar hello geografisk plats och status för alla hello fabriker i hello lösning.</span><span class="sxs-lookup"><span data-stu-id="73758-180">If your subscription has access toohello Bing Maps API, hello *Factories* map shows you hello geographical location and status of all hello factories in hello solution.</span></span> <span data-ttu-id="73758-181">toodrill hello plats detaljer Klicka hello platser som visas på kartan hello.</span><span class="sxs-lookup"><span data-stu-id="73758-181">toodrill into hello location details, click hello locations displayed on hello map.</span></span>

![Karta för den förkonfigurerade lösningen Ansluten fabrik][cf-img-map]

## <a name="view-alerts"></a><span data-ttu-id="73758-183">Visa aviseringar</span><span class="sxs-lookup"><span data-stu-id="73758-183">View alerts</span></span>

<span data-ttu-id="73758-184">Hej **avisering** panelen visas aviseringar på grund av tooa rapporterade värde eller ett beräknat OEE/KPI-värde som överskrider den konfigurerade tröskeln.</span><span class="sxs-lookup"><span data-stu-id="73758-184">hello **Alert** panel shows you alerts generated due tooa reported value or a calculated OEE/KPI value exceeding its configured threshold.</span></span> <span data-ttu-id="73758-185">Den här panelen visas aviseringar på varje nivå på hello hierarkin från hello station nivå toohello globala vyn.</span><span class="sxs-lookup"><span data-stu-id="73758-185">This panel displays alerts at each level of hello hierarchy, from hello station level view toohello global view.</span></span> <span data-ttu-id="73758-186">hello aviseringar innehåller en beskrivning av hello avisering, datum, tid, plats och antalet förekomster.</span><span class="sxs-lookup"><span data-stu-id="73758-186">hello alerts contain a description of hello alert, date, time, location, and number of occurrences.</span></span> <span data-ttu-id="73758-187">Du kan få insyn i toohello data som orsakade hello avisering med hello tid serien Insights-data.</span><span class="sxs-lookup"><span data-stu-id="73758-187">You can gain insights in toohello data that caused hello alert using hello Time Series Insights data.</span></span> <span data-ttu-id="73758-188">hello tid serien insikter data visualiseras i hello aviseringar om tillämpligt.</span><span class="sxs-lookup"><span data-stu-id="73758-188">hello Time Series Insights data is visualized in hello alerts where applicable.</span></span> <span data-ttu-id="73758-189">Om du är administratör kan koppla du standardåtgärder på hello aviseringar som:</span><span class="sxs-lookup"><span data-stu-id="73758-189">If you are an Administrator, you can take default actions on hello alerts such as:</span></span>

* <span data-ttu-id="73758-190">Stäng hello avisering.</span><span class="sxs-lookup"><span data-stu-id="73758-190">Close hello alert.</span></span>
* <span data-ttu-id="73758-191">Bekräfta hello avisering.</span><span class="sxs-lookup"><span data-stu-id="73758-191">Acknowledge hello alert.</span></span>

<span data-ttu-id="73758-192">Du kan också kan du utföra mer komplexa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="73758-192">Optionally, you can take more complex actions.</span></span> <span data-ttu-id="73758-193">För hello trycket OPC UA nod i hello sammansättningen kan du till exempel:</span><span class="sxs-lookup"><span data-stu-id="73758-193">For example, for hello Pressure OPC UA Node of hello Assembly you could:</span></span>

* <span data-ttu-id="73758-194">Visa mer information på en webbsida i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="73758-194">Show supporting information in a web page in a new browser window.</span></span>
* <span data-ttu-id="73758-195">Minska hello orsaken till hello avisering genom att anropa en metod för OPC UA på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="73758-195">Mitigate hello cause of hello alert by calling an OPC UA method on hello device.</span></span>
* <span data-ttu-id="73758-196">Utelämna hello tillgängligheten för hello standardåtgärder.</span><span class="sxs-lookup"><span data-stu-id="73758-196">Suppress hello availability of hello default actions.</span></span>

    ![Aviseringar för den förkonfigurerade lösningen Ansluten fabrik][cf-img-alerts]

> [!NOTE]
> <span data-ttu-id="73758-198">De här aviseringarna genereras av regler som har angetts i en konfigurationsfil i hello förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="73758-198">These alerts are generated by rules that are specified in a configuration file in hello preconfigured solution.</span></span> <span data-ttu-id="73758-199">De här reglerna kan generera aviseringar när hello OEE eller KPI siffror eller OPC UA nod värden överskrider den konfigurerade tröskeln.</span><span class="sxs-lookup"><span data-stu-id="73758-199">These rules can generate alerts when hello OEE or KPI figures or OPC UA Node values are exceeding their configured threshold.</span></span>

1. <span data-ttu-id="73758-200">Hej **aviseringar panelen** visar hello aviseringarna som skapats i den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="73758-200">hello **Alerts panel** shows hello alerts generated in this solution.</span></span>

2. <span data-ttu-id="73758-201">tooview hello information om en avisering på hello avisering hello aviseringar Kontrollpanelen.</span><span class="sxs-lookup"><span data-stu-id="73758-201">tooview hello details of an alert, click hello alert in hello alerts panel.</span></span>

3. <span data-ttu-id="73758-202">toofurther analysera hello aviseringsdata, klicka på hello diagram i hello avisering panelen tooopen hello tid serien insikter explorer miljö.</span><span class="sxs-lookup"><span data-stu-id="73758-202">toofurther analyze hello alert data, click hello graph in hello alert panel tooopen hello Time Series Insights explorer environment.</span></span>

4. <span data-ttu-id="73758-203">tooaddress Hej avisering, flera åtgärder är tillgängliga i hello avisering panelen.</span><span class="sxs-lookup"><span data-stu-id="73758-203">tooaddress hello alert, several actions are available in hello alert panel.</span></span> <span data-ttu-id="73758-204">Välj hello lämpligt alternativ för dig och på hello köra åtgärden kommandoknapp.</span><span class="sxs-lookup"><span data-stu-id="73758-204">Choose hello appropriate option for you and click hello execute action command button.</span></span>

## <a name="view-overall-equipment-efficiency"></a><span data-ttu-id="73758-205">Visa OEE (Overall Equipment Efficiency)</span><span class="sxs-lookup"><span data-stu-id="73758-205">View overall equipment efficiency</span></span>

<span data-ttu-id="73758-206">OEE priser hello effektiviteten för hello produktionsprocess med hjälp av en nyckel produktions-relaterade parametrar.</span><span class="sxs-lookup"><span data-stu-id="73758-206">OEE rates hello efficiency of hello manufacturing process using a key production-related operational parameters.</span></span> <span data-ttu-id="73758-207">OEE är en branschstandard som standard mått beräknas genom att multiplicera hello tillgänglighet hastighet, hastighet för prestanda och kvalitet snabbt: OEE = tillgänglighet x kvaliteten x.</span><span class="sxs-lookup"><span data-stu-id="73758-207">OEE is an industry standard measure calculated by multiplying hello availability rate, performance rate, and quality rate: OEE = availability x performance x quality.</span></span>

![OEE för den förkonfigurerade lösningen Ansluten fabrik][cf-img-oee]

1. <span data-ttu-id="73758-209">tooview OEE för en nivå i hierarkin hello, navigera toohello specifik vy som du behöver.</span><span class="sxs-lookup"><span data-stu-id="73758-209">tooview OEE for any level in hello hierarchy, navigate toohello specific view you require.</span></span> <span data-ttu-id="73758-210">Hej OEE för vyn visar hello Kontrollpanelen tillsammans med var och en av hello-element som utgör hello OEE procent.</span><span class="sxs-lookup"><span data-stu-id="73758-210">hello OEE for that view displays in hello panel along with each of hello elements that make up hello OEE percentage.</span></span>

2. <span data-ttu-id="73758-211">toofurther analysera hello OEE för en nivå i hello hierarkidata, klicka på hello OEE, tillgänglighet, prestanda och kvalitet procent.</span><span class="sxs-lookup"><span data-stu-id="73758-211">toofurther analyze hello OEE for any level in hello hierarchy data, click either hello OEE, availability, performance, or quality percentage.</span></span> <span data-ttu-id="73758-212">En kontext panel visas med tiden serien insikter driven visualiseringar som visar data från hello senaste timmen, senaste 24 timmarna och senaste 7 dagarna.</span><span class="sxs-lookup"><span data-stu-id="73758-212">A context panel appears with Time Series Insights powered visualizations that shows data from hello last hour, last 24 hours, and last 7 days.</span></span>

    ![TSI-visualisering för den förkonfigurerade lösningen Ansluten fabrik][cf-img-tsi-visualization]

3. <span data-ttu-id="73758-214">toofurther analysera hello aviseringsdata, klicka på hello diagrammet hello avisering Kontrollpanelen.</span><span class="sxs-lookup"><span data-stu-id="73758-214">toofurther analyze hello alert data, click hello graph in hello alert panel.</span></span> <span data-ttu-id="73758-215">Den här åtgärden öppnar hello tid serien insikter explorer miljö.</span><span class="sxs-lookup"><span data-stu-id="73758-215">This action opens hello Time Series Insights explorer environment.</span></span>

    ![TSI-utforskaren för den förkonfigurerade lösningen Ansluten fabrik][cf-img-tsi-explorer]

## <a name="view-key-performance-indicators"></a><span data-ttu-id="73758-217">Visa KPI: er</span><span class="sxs-lookup"><span data-stu-id="73758-217">View Key Performance Indicators</span></span>

<span data-ttu-id="73758-218">hello lösningen ger två nyckeltal *enheter per timme* och *energi som används i kWh*.</span><span class="sxs-lookup"><span data-stu-id="73758-218">hello solution provides two key performance indicators, *units per hour* and *energy used in kWh*.</span></span>

![KPI för den förkonfigurerade lösningen Ansluten fabrik][cf-img-kpi]

1. <span data-ttu-id="73758-220">tooview enheter per timme eller energi som används för alla nivåer i hierarkin hello, navigera toohello specifik vy som du behöver.</span><span class="sxs-lookup"><span data-stu-id="73758-220">tooview units per hour or energy used for any level in hello hierarchy, navigate toohello specific view you require.</span></span> <span data-ttu-id="73758-221">hello-enheter per timme och energi som används visas hello-panelen.</span><span class="sxs-lookup"><span data-stu-id="73758-221">hello units per hour and energy used display in hello panel.</span></span>

2. <span data-ttu-id="73758-222">tooanalyze enheter per timme eller energi som används för varje nivå i hierarkin för hello ytterligare, klicka på hello mätaren i hello **prestationsindikatorer** panelen.</span><span class="sxs-lookup"><span data-stu-id="73758-222">tooanalyze units per hour or energy used for any level in hello hierarchy further, click hello gauge in hello **Key Performance Indicators** panel.</span></span> <span data-ttu-id="73758-223">En kontext panel visas med tid serien insikter påslagen visualiseringar som gör att du tooview data från hello senaste timmen hello senaste 24 timmarna och senaste 7 dagarna.</span><span class="sxs-lookup"><span data-stu-id="73758-223">A context panel appears with Time Series Insights powered visualizations enabling you tooview data from hello last hour, hello last 24 hours, and last 7 days.</span></span>

## <a name="scenario-review"></a><span data-ttu-id="73758-224">Scenariogranskning</span><span class="sxs-lookup"><span data-stu-id="73758-224">Scenario review</span></span>

<span data-ttu-id="73758-225">I det här scenariot övervakade din fabriker OEE och KPI: er värden i hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="73758-225">In this scenario, you monitored your factories OEE and KPIs values, in hello dashboard.</span></span> <span data-ttu-id="73758-226">Du sedan för tid serien insikter tooprovide mer information toohelp detaljerat ytterligare hello telemetridata för OEE och KPI: er toohelp med upptäcka avvikelser.</span><span class="sxs-lookup"><span data-stu-id="73758-226">You then used Time Series Insights tooprovide more information toohelp drill further into hello telemetry data for OEE and KPIs toohelp with detecting anomalies.</span></span> <span data-ttu-id="73758-227">Du också används hello avisering panelen tooview problem med din fabriker och du använde hello åtgärder tillgängliga tooyou tooresolve hello avisering.</span><span class="sxs-lookup"><span data-stu-id="73758-227">You also used hello alert panel tooview issues with your factories and you used hello actions available tooyou tooresolve hello alert.</span></span>

## <a name="other-features"></a><span data-ttu-id="73758-228">Andra funktioner</span><span class="sxs-lookup"><span data-stu-id="73758-228">Other features</span></span>

<span data-ttu-id="73758-229">hello följande avsnitt beskrivs några ytterligare funktionerna i hello anslutna factory lösning som inte beskrivs i föregående hello-scenariot.</span><span class="sxs-lookup"><span data-stu-id="73758-229">hello following sections describe some additional features of hello connected factory solution that are not described in hello previous scenario.</span></span>

## <a name="apply-filters"></a><span data-ttu-id="73758-230">Använda filter</span><span class="sxs-lookup"><span data-stu-id="73758-230">Apply filters</span></span>

1. <span data-ttu-id="73758-231">Klicka på hello **ikonen** toodisplay en lista över tillgängliga filter på antingen hello factory platser panelen eller hello aviseringar.</span><span class="sxs-lookup"><span data-stu-id="73758-231">Click hello **chevron** toodisplay a list of available filters in either hello factory locations panel or hello alerts panel.</span></span>

2. <span data-ttu-id="73758-232">hello filterpanelen visas för dig.</span><span class="sxs-lookup"><span data-stu-id="73758-232">hello filters panel is displayed for you.</span></span> 

    ![Filter för den förkonfigurerade lösningen Ansluten fabrik][cf-img-alert-filter]

3. <span data-ttu-id="73758-234">Välj hello filter som du behöver.</span><span class="sxs-lookup"><span data-stu-id="73758-234">Choose hello filter that you require.</span></span> <span data-ttu-id="73758-235">Det är också möjligt tootype fritext i hello filter-fält.</span><span class="sxs-lookup"><span data-stu-id="73758-235">It is also possible tootype free text into hello filter fields.</span></span>

4. <span data-ttu-id="73758-236">hello-filter används sedan för dig.</span><span class="sxs-lookup"><span data-stu-id="73758-236">hello filter is then applied for you.</span></span> <span data-ttu-id="73758-237">hello Filterläge visas också i hello instrumentpanel via en Trattens som visar hello fabriker och aviseringar tabeller.</span><span class="sxs-lookup"><span data-stu-id="73758-237">hello filter state is also shown in hello dashboard via a funnel that displays in hello factories and alerts tables.</span></span>

    ![Filter för den förkonfigurerade lösningen Ansluten fabrik][cf-img-alert-filter-funnel]

    > [!NOTE]
    > <span data-ttu-id="73758-239">Ett aktivt filter påverkar inte hello visas OEE och KPI värden endast filtrerar hello lista innehåll.</span><span class="sxs-lookup"><span data-stu-id="73758-239">An active filter does not affect hello displayed OEE and KPI values, it only filters hello list contents.</span></span>

5. <span data-ttu-id="73758-240">tooclear ett filter, klicka på hello Trattens och filter i hello filterpanel kontext.</span><span class="sxs-lookup"><span data-stu-id="73758-240">tooclear a filter, click hello funnel and click filter in hello filter context panel.</span></span> <span data-ttu-id="73758-241">Hej text **alla** visas i hello fabriker och aviseringar tabeller.</span><span class="sxs-lookup"><span data-stu-id="73758-241">hello text **All** is displayed in hello factories and alerts tables.</span></span>

## <a name="browse-an-opc-ua-server"></a><span data-ttu-id="73758-242">Navigera i en OPC UA-serverläsare</span><span class="sxs-lookup"><span data-stu-id="73758-242">Browse an OPC UA server</span></span>

<span data-ttu-id="73758-243">När du distribuerar hello förkonfigurerade lösningen kan etablera du automatiskt simulerade OPC UA-servrar som du kan bläddra via hello lösning webbläsare.</span><span class="sxs-lookup"><span data-stu-id="73758-243">When you deploy hello preconfigured solution, you automatically provision simulated OPC UA servers that you can browse via hello solution browser.</span></span> <span data-ttu-id="73758-244">Dessa servrar är *simulerade OPC UA-servrar*.</span><span class="sxs-lookup"><span data-stu-id="73758-244">These servers are *simulated OPC UA servers*.</span></span> <span data-ttu-id="73758-245">Simulerade servrar gör det enkelt för dig tooexperiment med hello förkonfigurerade lösningen utan hello måste toodeploy verkliga fysiska servrar.</span><span class="sxs-lookup"><span data-stu-id="73758-245">Simulated servers make it easy for you tooexperiment with hello preconfigured solution without hello need toodeploy real, physical servers.</span></span> <span data-ttu-id="73758-246">Om du vill tooconnect en verklig OPC UA toohello serverlösning, se hello [ansluta OPC UA enheten toohello anslutna factory förkonfigurerade lösningen] [ lnk-connect-cf] kursen.</span><span class="sxs-lookup"><span data-stu-id="73758-246">If you do want tooconnect a real OPC UA server toohello solution, see hello [Connect your OPC UA device toohello connected factory preconfigured solution][lnk-connect-cf] tutorial.</span></span>

1. <span data-ttu-id="73758-247">Klicka på hello **factory ikonen** i navigeringsfältet i hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="73758-247">Click hello **factory icon** in hello dashboard navigation bar.</span></span>

    ![Serverläsare för den förkonfigurerade lösningen Ansluten fabrik][cf-img-server-browser]

2. <span data-ttu-id="73758-249">Välj en av servrarna hello hello förkonfigurerade listan.</span><span class="sxs-lookup"><span data-stu-id="73758-249">Choose one of hello servers from hello preconfigured list.</span></span> <span data-ttu-id="73758-250">Den här listan visar hello-servrar som distribueras i hello förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="73758-250">This list shows hello servers that are deployed for you in hello preconfigured solution.</span></span>

    ![Serverurval för den förkonfigurerade lösningen Ansluten fabrik][cf-img-server-choice]

3. <span data-ttu-id="73758-252">Klicka på **Connect** (Anslut) för att visa en dialogruta för säkerhet.</span><span class="sxs-lookup"><span data-stu-id="73758-252">Click **Connect**, a security dialog displays.</span></span> <span data-ttu-id="73758-253">Hello simuleringen, är det säkra tooclick **Fortsätt**</span><span class="sxs-lookup"><span data-stu-id="73758-253">For hello simulation, it is safe tooclick **Proceed**</span></span>

4. <span data-ttu-id="73758-254">tooexpand någon av hello noderna i trädet för hello server klickar du på den.</span><span class="sxs-lookup"><span data-stu-id="73758-254">tooexpand any of hello nodes in hello server tree, click it.</span></span> <span data-ttu-id="73758-255">Noder som publicerar telemetri har ett skalstreck bredvid sig.</span><span class="sxs-lookup"><span data-stu-id="73758-255">Nodes that are publishing telemetry have a tick mark beside them.</span></span>

    ![Serverträd för den förkonfigurerade lösningen Ansluten fabrik][cf-img-server-tree]

5. <span data-ttu-id="73758-257">Högerklicka på ett objekt tooread, skriva, publicera eller anropa noden.</span><span class="sxs-lookup"><span data-stu-id="73758-257">Right-click an item tooread, write, publish, or call that node.</span></span> <span data-ttu-id="73758-258">hello åtgärder tillgängliga tooyou beror på dina behörigheter och hello attribut för hello-nod.</span><span class="sxs-lookup"><span data-stu-id="73758-258">hello actions available tooyou depend on your permissions and hello attributes of hello node.</span></span> <span data-ttu-id="73758-259">hello läsa alternativet toodisplays en kontext panel visar hello värdet hello specifik nod.</span><span class="sxs-lookup"><span data-stu-id="73758-259">hello read option toodisplays a context panel showing hello value of hello specific node.</span></span> <span data-ttu-id="73758-260">hello skriva alternativet visar en kontext panel där du kan ange ett nytt värde.</span><span class="sxs-lookup"><span data-stu-id="73758-260">hello write option displays a context panel where you can enter a new value.</span></span> <span data-ttu-id="73758-261">Hej samtalsalternativet visar en nod där du kan ange hello parametrar för hello-anrop.</span><span class="sxs-lookup"><span data-stu-id="73758-261">hello call option displays a node where you can enter hello parameters for hello call.</span></span>

## <a name="publish-a-node"></a><span data-ttu-id="73758-262">Publicera en nod</span><span class="sxs-lookup"><span data-stu-id="73758-262">Publish a node</span></span>

<span data-ttu-id="73758-263">När du bläddrar en *simulerade OPC UA server*, du kan också välja toopublish nya noder.</span><span class="sxs-lookup"><span data-stu-id="73758-263">When you browse a *simulated OPC UA server*, you can also choose toopublish new nodes.</span></span> <span data-ttu-id="73758-264">Du kan analysera hello telemetri från dessa noder i hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="73758-264">You can analyze hello telemetry from these nodes in hello solution.</span></span> <span data-ttu-id="73758-265">Dessa *simulerade OPC UA servrar* gör det enkelt tooexperiment med hello förkonfigurerade lösningen utan att distribuera verkliga fysiska enheter.</span><span class="sxs-lookup"><span data-stu-id="73758-265">These *simulated OPC UA servers* make it easy tooexperiment with hello preconfigured solution without deploying real, physical devices.</span></span>

1. <span data-ttu-id="73758-266">Bläddra tooa nod i hello OPC UA webbläsare serverträdet toopublish gärna.</span><span class="sxs-lookup"><span data-stu-id="73758-266">Browse tooa node in hello OPC UA server browser tree that you wish toopublish.</span></span>

2. <span data-ttu-id="73758-267">Högerklicka på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="73758-267">Right-click hello node.</span></span>

3. <span data-ttu-id="73758-268">Välj **Publish** (Publicera).</span><span class="sxs-lookup"><span data-stu-id="73758-268">Choose **Publish**.</span></span>

    ![Ansluten fabrik publicerar nod][cf-img-publish-node]

4. <span data-ttu-id="73758-270">En kontext ruta visas som talar om att hello publicera har slutförts.</span><span class="sxs-lookup"><span data-stu-id="73758-270">A context panel appears which tells you that hello publish has succeeded.</span></span> <span data-ttu-id="73758-271">hello noden visas på hello station nivån med en bock bredvid den.</span><span class="sxs-lookup"><span data-stu-id="73758-271">hello node appears in hello station level view with a check mark beside it.</span></span>

    ![Slutförd publicering i den förkonfigurerade lösningen Ansluten fabrik][cf-img-publish-success]

## <a name="command-and-control"></a><span data-ttu-id="73758-273">Kommando och kontroll</span><span class="sxs-lookup"><span data-stu-id="73758-273">Command and control</span></span>

<span data-ttu-id="73758-274">hello anslutna fabriken kan du kommandot och kontrollera dina branschen enheter direkt från hello molnet.</span><span class="sxs-lookup"><span data-stu-id="73758-274">hello connected factory allows you command and control your industry devices directly from hello cloud.</span></span> <span data-ttu-id="73758-275">Du kan använda den här funktionen toorespond tooalerts som genererats av hello enhet.</span><span class="sxs-lookup"><span data-stu-id="73758-275">You can use this feature toorespond tooalerts generated by hello device.</span></span> <span data-ttu-id="73758-276">Du kan till exempel skicka en kommandot toohello enhet från hello molnet.</span><span class="sxs-lookup"><span data-stu-id="73758-276">For example, you could send a command toohello device from hello cloud.</span></span> <span data-ttu-id="73758-277">Du hittar hello tillgängliga kommandon i hello **StationCommands** nod i hello OPC UA servrar webbläsarträdet.</span><span class="sxs-lookup"><span data-stu-id="73758-277">You can find hello available commands in hello **StationCommands** node in hello OPC UA servers browser tree.</span></span> <span data-ttu-id="73758-278">Öppna en trycket versionen används på hello sammansättningen station av en rad för produktion i München i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="73758-278">In this scenario, you open a pressure release valve on hello assembly station of a production line in Munich.</span></span> <span data-ttu-id="73758-279">toouse hello-kommando- och funktioner, måste du vara i hello **administratör** roll för hello förkonfigurerade lösningsdistribution.</span><span class="sxs-lookup"><span data-stu-id="73758-279">toouse hello command and control functionality, you must be in hello **Administrator** role for hello preconfigured solution deployment.</span></span>

1. <span data-ttu-id="73758-280">Bläddra toohello **StationCommands** nod i hello OPC UA serverträdet webbläsare.</span><span class="sxs-lookup"><span data-stu-id="73758-280">Browse toohello **StationCommands** node in hello OPC UA server browser tree.</span></span>

2. <span data-ttu-id="73758-281">Välj hello-kommandot som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="73758-281">Choose hello command that you wish use.</span></span>

3. <span data-ttu-id="73758-282">Högerklicka på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="73758-282">Right-click hello node.</span></span>

4. <span data-ttu-id="73758-283">Välj **Call** (Anropa).</span><span class="sxs-lookup"><span data-stu-id="73758-283">Choose **Call**.</span></span>

    ![Anropskommando för den förkonfigurerade lösningen Ansluten fabrik][cf-img-call-command]

5. <span data-ttu-id="73758-285">En kontext ruta visas information om vilken metod du toocall och eventuella parameterinformation gäller.</span><span class="sxs-lookup"><span data-stu-id="73758-285">A context panel appears informing you which method you are about toocall and any parameter details is applicable.</span></span>

6. <span data-ttu-id="73758-286">Välj **Call** (Anropa).</span><span class="sxs-lookup"><span data-stu-id="73758-286">Choose **Call**.</span></span>

    ![Kontextpanel för anrop för den förkonfigurerade lösningen Ansluten fabrik][cf-img-call-context]

7. <span data-ttu-id="73758-288">hello kontexten panelen är uppdaterade tooinform att hello-metodanrop lyckades.</span><span class="sxs-lookup"><span data-stu-id="73758-288">hello context panel is updated tooinform you that hello method call succeeded.</span></span> <span data-ttu-id="73758-289">Du kan kontrollera hello anropet lyckades genom att läsa hello värdet för hello trycket nod som har uppdaterats på grund av hello-anrop.</span><span class="sxs-lookup"><span data-stu-id="73758-289">You can verify hello call succeeded by reading hello value of hello pressure node that updated as a result of hello call.</span></span>

    ![Slutfört anrop för den förkonfigurerade lösningen Ansluten fabrik][cf-img-call-success]


## <a name="behind-hello-scenes"></a><span data-ttu-id="73758-291">Hello bakgrunden</span><span class="sxs-lookup"><span data-stu-id="73758-291">Behind hello scenes</span></span>

<span data-ttu-id="73758-292">När du distribuerar en förkonfigurerade lösning skapar hello distributionsprocessen flera resurser i hello Azure-prenumeration du valt.</span><span class="sxs-lookup"><span data-stu-id="73758-292">When you deploy a preconfigured solution, hello deployment process creates multiple resources in hello Azure subscription you selected.</span></span> <span data-ttu-id="73758-293">Du kan visa dessa resurser i hello Azure [portal][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="73758-293">You can view these resources in hello Azure [portal][lnk-portal].</span></span> <span data-ttu-id="73758-294">hello distributionsprocessen skapar en **resursgruppen** med ett namn baserat på hello namn du väljer för din förkonfigurerade lösning:</span><span class="sxs-lookup"><span data-stu-id="73758-294">hello deployment process creates a **resource group** with a name based on hello name you choose for your preconfigured solution:</span></span>

![Förkonfigurerade lösning i hello Azure-portalen][img-cf-portal]

<span data-ttu-id="73758-296">Du kan visa hello inställningarna för varje resurs som väljs i hello listan över resurser i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="73758-296">You can view hello settings of each resource by selecting it in hello list of resources in hello resource group.</span></span>

<span data-ttu-id="73758-297">Du kan också visa hello källkoden för hello förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="73758-297">You can also view hello source code for hello preconfigured solution.</span></span> <span data-ttu-id="73758-298">hello källkoden för anslutna factory förkonfigurerade lösningen finns i hello [azure iot-ansluten-fabrik] [ lnk-cfgithub] GitHub-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="73758-298">hello connected factory preconfigured solution source code is in hello [azure-iot-connected-factory][lnk-cfgithub] GitHub repository:</span></span>

<span data-ttu-id="73758-299">När du är klar kan du ta bort hello förkonfigurerade lösningen från din Azure-prenumeration på hello [azureiotsuite.com] [ lnk-azureiotsuite] plats.</span><span class="sxs-lookup"><span data-stu-id="73758-299">When you are done, you can delete hello preconfigured solution from your Azure subscription on hello [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="73758-300">Den här webbplatsen kan tooeasily ta bort alla hello resurser som etablerades när du skapade hello förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="73758-300">This site enables you tooeasily delete all hello resources that were provisioned when you created hello preconfigured solution.</span></span>

> [!NOTE]
> <span data-ttu-id="73758-301">tooensure att du tar bort allt relaterade toohello förkonfigurerade lösningen kan ta bort den på hello [azureiotsuite.com] [ lnk-azureiotsuite] plats.</span><span class="sxs-lookup"><span data-stu-id="73758-301">tooensure that you delete everything related toohello preconfigured solution, delete it on hello [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="73758-302">Ta inte bort hello resursgrupp i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="73758-302">Do not delete hello resource group in hello portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73758-303">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73758-303">Next Steps</span></span>

<span data-ttu-id="73758-304">Nu när du har distribuerat en fungerande förkonfigurerade lösning kan fortsätta du komma igång med IoT Suite genom att läsa hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="73758-304">Now that you’ve deployed a working preconfigured solution, you can continue getting started with IoT Suite by reading hello following articles:</span></span>

* <span data-ttu-id="73758-305">[Genomgång av den förkonfigurerade lösningen Ansluten fabrik][lnk-rm-walkthrough]</span><span class="sxs-lookup"><span data-stu-id="73758-305">[Connected factory preconfigured solution walkthrough][lnk-rm-walkthrough]</span></span>
* <span data-ttu-id="73758-306">[Ansluta enheten toohello anslutna factory förkonfigurerade lösningen][lnk-connect-cf]</span><span class="sxs-lookup"><span data-stu-id="73758-306">[Connect your device toohello Connected factory preconfigured solution][lnk-connect-cf]</span></span>
* <span data-ttu-id="73758-307">[Behörigheter för hello azureiotsuite.com plats][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="73758-307">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>

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