---
title: "aaaPredictive Underhåll förkonfigurerade lösningen | Microsoft Docs"
description: "En beskrivning av hello Azure IoT Suite förutsägande Underhåll förkonfigurerade lösningen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: b370b3d7-2ce5-4906-9818-3aeedd471ee3
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 2d09801467d33db6b7d6333fa071aea2bf573f20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="predictive-maintenance-preconfigured-solution-overview"></a><span data-ttu-id="3f036-103">Översikt över förkonfigurerad lösning för förebyggande underhåll</span><span class="sxs-lookup"><span data-stu-id="3f036-103">Predictive maintenance preconfigured solution overview</span></span>

<span data-ttu-id="3f036-104">Hej *förutsägande Underhåll* [förkonfigurerade lösningen] [ lnk_preconfigured_solutions] är en av hello [Microsoft Azure IoT Suite] [ lnk_iot_suite] förkonfigurerade lösningar.</span><span class="sxs-lookup"><span data-stu-id="3f036-104">hello *predictive maintenance* [preconfigured solution][lnk_preconfigured_solutions] is one of hello [Microsoft Azure IoT Suite][lnk_iot_suite] preconfigured solutions.</span></span> <span data-ttu-id="3f036-105">Den här lösningen integrerar realtidsinsamling av enhetstelemetri med en förebyggande modell skapad med [Azure Machine Learning][lnk-machine-learning].</span><span class="sxs-lookup"><span data-stu-id="3f036-105">This solution integrates real-time device telemetry collection with a predictive model created using [Azure Machine Learning][lnk-machine-learning].</span></span>

<span data-ttu-id="3f036-106">Med Azure IoT Suite du snabbt och enkelt ansluta tooand övervakaren tillgångar och analysera telemetri i realtid i instrumentpaneler och visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="3f036-106">With Azure IoT Suite, you can quickly and easily connect tooand monitor assets, and analyze telemetry in real time in dashboards and visualizations.</span></span> <span data-ttu-id="3f036-107">I hello förutsägande underhållslösningen ger hello instrumentpaneler och visualiseringar nya intelligence som kan enheten effektivitet och förbättra intäkter dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="3f036-107">In hello predictive maintenance solution, hello dashboards and visualizations provide you with new intelligence that can drive efficiencies and enhance revenue streams.</span></span>

## <a name="hello-scenario"></a><span data-ttu-id="3f036-108">hello Scenario</span><span class="sxs-lookup"><span data-stu-id="3f036-108">hello Scenario</span></span>

<span data-ttu-id="3f036-109">Fabrikam är ett regionalt flygbolag som fokuserar på bra kundupplevelser till konkurrenskraftiga priser.</span><span class="sxs-lookup"><span data-stu-id="3f036-109">Fabrikam is a regional airline that focuses on great customer experience at competitive prices.</span></span> <span data-ttu-id="3f036-110">En orsak till flygförseningar är underhållsproblem, och flygplansmotorernas underhåll är särskilt krävande.</span><span class="sxs-lookup"><span data-stu-id="3f036-110">One cause of flight delays is maintenance issues and aircraft engine maintenance is particularly challenging.</span></span> <span data-ttu-id="3f036-111">Fabrikam måste undvika motorn fel under svarta på alla vis så att den regelbundet kontrollerar dess motorer och schemalägger underhåll enligt tooa plan.</span><span class="sxs-lookup"><span data-stu-id="3f036-111">Fabrikam must avoid engine failure during flight at all costs, so it inspects its engines regularly and schedules maintenance according tooa plan.</span></span> <span data-ttu-id="3f036-112">Dock hello flygplan motorer inte bär alltid densamma.</span><span class="sxs-lookup"><span data-stu-id="3f036-112">However, aircraft engines don’t always wear hello same.</span></span> <span data-ttu-id="3f036-113">En del onödigt underhåll utförs på motorerna.</span><span class="sxs-lookup"><span data-stu-id="3f036-113">Some unnecessary maintenance is performed on engines.</span></span> <span data-ttu-id="3f036-114">Dessutom kan det uppstå problem som gör att planet blir stående tills underhåll har utförts.</span><span class="sxs-lookup"><span data-stu-id="3f036-114">More importantly, issues arise which can ground an aircraft until maintenance is performed.</span></span> <span data-ttu-id="3f036-115">Om ett flygplan är på en plats hello där rätt tekniker eller delar är inte tillgänglig, dessa problem kan vara särskilt kostsamma.</span><span class="sxs-lookup"><span data-stu-id="3f036-115">If an aircraft is at a location where hello right technicians or spare parts are not available, these issues can be especially costly.</span></span>

<span data-ttu-id="3f036-116">hello motorer i Fabrikams flygplan instrumenterats med sensorer som övervakar motorn villkor under svarta.</span><span class="sxs-lookup"><span data-stu-id="3f036-116">hello engines of Fabrikam’s aircraft are instrumented with sensors that monitor engine conditions during flight.</span></span> <span data-ttu-id="3f036-117">Fabrikam använder hello förutsägande Underhåll lösningen toocollect hello sensordata som samlas in under hello svarta.</span><span class="sxs-lookup"><span data-stu-id="3f036-117">Fabrikam uses hello predictive maintenance solution toocollect hello sensor data collected during hello flight.</span></span> <span data-ttu-id="3f036-118">När kan sparas års motorn operativa och fel data har Fabrikams datavetare modelleras en sätt toopredict hello återstående livslängd (RUL) ett flygplan-motorn.</span><span class="sxs-lookup"><span data-stu-id="3f036-118">After accumulating years of engine operational and failure data, Fabrikam’s data scientists have modeled a way toopredict hello Remaining Useful Life (RUL) of an aircraft engine.</span></span> <span data-ttu-id="3f036-119">hello modellen använder en korrelation mellan data från fyra hello motorn sensorer och motorn förslitning som leder tooeventual fel.</span><span class="sxs-lookup"><span data-stu-id="3f036-119">hello model uses a correlation between data from four of hello engine sensors and engine wear that leads tooeventual failure.</span></span> <span data-ttu-id="3f036-120">Medan Fabrikam fortsätter tooperform regelbundna kontroller tooensure säkerhet, kan nu använda hello modeller toocompute hello RUL för varje motor efter varje svarta.</span><span class="sxs-lookup"><span data-stu-id="3f036-120">While Fabrikam continues tooperform regular inspections tooensure safety, it can now use hello models toocompute hello RUL for each engine after every flight.</span></span> <span data-ttu-id="3f036-121">hello modellen använder hello telemetri som samlats in från hello motorer under hello svarta.</span><span class="sxs-lookup"><span data-stu-id="3f036-121">hello model uses hello telemetry collected from hello engines during hello flight.</span></span> <span data-ttu-id="3f036-122">Fabrikam kan nu förutsäga framtida tidpunkter för potentiella fel och planera för underhåll och reparation i förväg.</span><span class="sxs-lookup"><span data-stu-id="3f036-122">Fabrikam can now predict future points of failure and plan for maintenance and repair in advance.</span></span>

> [!NOTE]
> <span data-ttu-id="3f036-123">hello lösning modellen använder motorns verkliga förslitning data.</span><span class="sxs-lookup"><span data-stu-id="3f036-123">hello solution model uses actual engine wear data.</span></span>

<span data-ttu-id="3f036-124">Fabrikam kan optimera sina åtgärder tooreduce kostnader genom att förutsäga hello punkt när Underhåll krävs.</span><span class="sxs-lookup"><span data-stu-id="3f036-124">By predicting hello point when maintenance is required, Fabrikam can optimize its operations tooreduce costs.</span></span>

<span data-ttu-id="3f036-125">Underhållskoordinatorer arbetar tillsammans med schemaläggare för att:</span><span class="sxs-lookup"><span data-stu-id="3f036-125">Maintenance coordinators work with schedulers to:</span></span>

- <span data-ttu-id="3f036-126">Planera Underhåll toocoincide med ett flygplan stoppas på en viss plats.</span><span class="sxs-lookup"><span data-stu-id="3f036-126">Plan maintenance toocoincide with an aircraft stopping at a particular location.</span></span>
- <span data-ttu-id="3f036-127">Se till att tillräckligt med tid är tillgänglig för hello flygplan toobe ur funktion utan att orsaka avbrott i schemat.</span><span class="sxs-lookup"><span data-stu-id="3f036-127">Ensure sufficient time is available for hello aircraft toobe out of service without causing schedule disruption.</span></span>
- <span data-ttu-id="3f036-128">tooschedule tekniker tooensure att flygplan går effektivt utan väntetid.</span><span class="sxs-lookup"><span data-stu-id="3f036-128">tooschedule technicians tooensure that aircraft are serviced efficiently without wait time.</span></span>

<span data-ttu-id="3f036-129">Inventeringskontrollchefer får underhållsplaner så att de kan optimera beställningsprocessen och reservdelslagret.</span><span class="sxs-lookup"><span data-stu-id="3f036-129">Inventory control managers receive maintenance plans, so they can optimize their ordering process and spare parts inventory.</span></span>

<span data-ttu-id="3f036-130">Dessa aktiviteter aktivera Fabrikam toominimize flygplan grunden tid och minska kostnaderna samtidigt hello passagerare och säkerhet teamet.</span><span class="sxs-lookup"><span data-stu-id="3f036-130">These activities enable Fabrikam toominimize aircraft ground time and reduce operating costs while ensuring hello safety of passengers and crew.</span></span>

<span data-ttu-id="3f036-131">toounderstand hur [Azure IoT Suite] [ lnk_iot_suite] ger hello funktioner kunder måste toorealize hello potentiella förutsägande Underhåll igenom detta [infographic] [lnk_infographic].</span><span class="sxs-lookup"><span data-stu-id="3f036-131">toounderstand how [Azure IoT Suite][lnk_iot_suite] provides hello capabilities customers need toorealize hello potential of predictive maintenance, review this [infographic][lnk_infographic].</span></span>

## <a name="how-hello-predictive-maintenance-solution-is-built"></a><span data-ttu-id="3f036-132">Hur hello förutsägande underhållslösningen byggs</span><span class="sxs-lookup"><span data-stu-id="3f036-132">How hello predictive maintenance solution is built</span></span>

<span data-ttu-id="3f036-133">hello lösningen använder en befintlig Azure Machine Learning-modell tillgänglig som en mall tooshow dessa funktioner fungerar från enheten telemetri som samlats in via IoT Suite-tjänster.</span><span class="sxs-lookup"><span data-stu-id="3f036-133">hello solution uses an existing Azure Machine Learning model available as a template tooshow these capabilities working from device telemetry collected through IoT Suite services.</span></span> <span data-ttu-id="3f036-134">Microsoft har skapat en [regressionsmodell] [ lnk_regression_model] av flygplan motor baserat på offentligt tillgängliga data<sup>\[1\]</sup>, och stegvisa information om hur toouse hello modellen.</span><span class="sxs-lookup"><span data-stu-id="3f036-134">Microsoft has built a [regression model][lnk_regression_model] of an aircraft engine based on publically available data<sup>\[1\]</sup>, and step-by-step guidance on how toouse hello model.</span></span>

<span data-ttu-id="3f036-135">hello Azure IoT förutsägande underhållslösningen används hello regressionsmodell som skapas utifrån mallen.</span><span class="sxs-lookup"><span data-stu-id="3f036-135">hello Azure IoT predictive maintenance solution uses hello regression model created from this template.</span></span> <span data-ttu-id="3f036-136">hello-modellen har distribuerats till din Azure-prenumeration och exponeras via en automatiskt genererad API.</span><span class="sxs-lookup"><span data-stu-id="3f036-136">hello model is deployed into your Azure subscription and exposed through an automatically generated API.</span></span> <span data-ttu-id="3f036-137">hello lösningen innehåller en delmängd av hello testa data som representerar 4 (i 100 totala) motorer och hello 4 (av totalt 21) sensor dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="3f036-137">hello solution includes a subset of hello testing data representing 4 (of 100 total) engines and hello 4 (of 21 total) sensor data streams.</span></span> <span data-ttu-id="3f036-138">Dessa data är tillräckligt tooprovide en korrekt resultat från hello tränad modell.</span><span class="sxs-lookup"><span data-stu-id="3f036-138">This data is sufficient tooprovide an accurate result from hello trained model.</span></span>

<span data-ttu-id="3f036-139">*\[1\] A. Saxena och K. Goebel (2008). "Turbofan Engine Degradation Simulation Data Set", NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*</span><span class="sxs-lookup"><span data-stu-id="3f036-139">*\[1\] A. Saxena and K. Goebel (2008). "Turbofan Engine Degradation Simulation Data Set", NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*</span></span>

## <a name="get-started-with-predictive-maintenance"></a><span data-ttu-id="3f036-140">Kom igång med förebyggande underhåll</span><span class="sxs-lookup"><span data-stu-id="3f036-140">Get started with predictive maintenance</span></span>

<span data-ttu-id="3f036-141">Den här kursen visar hur tooprovision hello förutsägande Underhåll lösning.</span><span class="sxs-lookup"><span data-stu-id="3f036-141">This tutorial shows you how tooprovision hello predictive maintenance solution.</span></span> <span data-ttu-id="3f036-142">Även vägleder dig genom hello grundläggande funktioner i hello förutsägande Underhåll lösning.</span><span class="sxs-lookup"><span data-stu-id="3f036-142">It also walks you through hello basic features of hello predictive maintenance solution.</span></span> <span data-ttu-id="3f036-143">Du kan komma åt många av dessa funktioner via hello lösning instrumentpanelen som distribuerar tillsammans med hello förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="3f036-143">You can access many of these features through hello solution dashboard that deploys along with hello preconfigured solution.</span></span>

<span data-ttu-id="3f036-144">toocomplete den här självstudiekursen kommer du behöver en aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3f036-144">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="3f036-145">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="3f036-145">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="3f036-146">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="3f036-146">For details, see [Azure Free Trial][lnk_free_trial].</span></span>

1. <span data-ttu-id="3f036-147">Logga in för[azureiotsuite.com] [ lnk-azureiotsuite] med din Azure kontoautentiseringsuppgifter och klicka på ** + ** toocreate en lösning.</span><span class="sxs-lookup"><span data-stu-id="3f036-147">Log on too[azureiotsuite.com][lnk-azureiotsuite] using your Azure account credentials, and click **+** toocreate a solution.</span></span>
1. <span data-ttu-id="3f036-148">Klicka på **Välj** hello **förutsägande Underhåll** panelen.</span><span class="sxs-lookup"><span data-stu-id="3f036-148">Click **Select** hello **Predictive maintenance** tile.</span></span>
1. <span data-ttu-id="3f036-149">Ange ett **lösningsnamn** för din förkonfigurerade lösning för förebyggande underhåll.</span><span class="sxs-lookup"><span data-stu-id="3f036-149">Enter a **Solution name** for your predictive maintenance preconfigured solution.</span></span>
1. <span data-ttu-id="3f036-150">Välj hello **Region** och **prenumeration** du vill toouse tooprovision hello-lösningen.</span><span class="sxs-lookup"><span data-stu-id="3f036-150">Select hello **Region** and **Subscription** you want toouse tooprovision hello solution.</span></span>
1. <span data-ttu-id="3f036-151">Klicka på **skapa lösningen** toobegin hello etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="3f036-151">Click **Create Solution** toobegin hello provisioning process.</span></span> <span data-ttu-id="3f036-152">Denna process brukar ta flera minuter toorun.</span><span class="sxs-lookup"><span data-stu-id="3f036-152">This process typically takes several minutes toorun.</span></span>

### <a name="wait-for-hello-provisioning-process-toocomplete"></a><span data-ttu-id="3f036-153">Vänta tills hello etablering processen toocomplete</span><span class="sxs-lookup"><span data-stu-id="3f036-153">Wait for hello provisioning process toocomplete</span></span>

1. <span data-ttu-id="3f036-154">Klicka på hello panelen för din lösning med **etablering** status.</span><span class="sxs-lookup"><span data-stu-id="3f036-154">Click hello tile for your solution with **Provisioning** status.</span></span>
1. <span data-ttu-id="3f036-155">Meddelande hello **etablering tillstånd** som Azure-tjänster har distribuerats i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3f036-155">Notice hello **Provisioning states** as Azure services are deployed in your Azure subscription.</span></span>
1. <span data-ttu-id="3f036-156">När etableringen är klar hello status ändras för**klar**.</span><span class="sxs-lookup"><span data-stu-id="3f036-156">Once provisioning completes, hello status changes too**Ready**.</span></span>
1. <span data-ttu-id="3f036-157">Klicka på panelen hello toosee hello information i lösningen i hello högra rutan.</span><span class="sxs-lookup"><span data-stu-id="3f036-157">Click hello tile toosee hello details of your solution in hello right-hand pane.</span></span> <span data-ttu-id="3f036-158">Från det här fönstret kan du starta hello lösning instrumentpanel och åtkomst hello Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="3f036-158">From this pane, you can launch hello solution dashboard and access hello Machine Learning workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="3f036-159">Om du får problem som distribuerar hello förkonfigurerade lösningen granska [behörigheter på hello azureiotsuite.com platsen] [ lnk-permissions] och hello [vanliga frågor och svar] [ lnk-faq].</span><span class="sxs-lookup"><span data-stu-id="3f036-159">If you encounter issues deploying hello preconfigured solution, review [Permissions on hello azureiotsuite.com site][lnk-permissions] and hello [FAQ][lnk-faq].</span></span> <span data-ttu-id="3f036-160">Om hello problem kvarstår, skapa en tjänstbiljett på hello [portal][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="3f036-160">If hello issues persist, create a service ticket on hello [portal][lnk-portal].</span></span>

<span data-ttu-id="3f036-161">Finns det information som du förväntar dig toosee som inte visas för lösningen?</span><span class="sxs-lookup"><span data-stu-id="3f036-161">Are there details you'd expect toosee that aren't listed for your solution?</span></span> <span data-ttu-id="3f036-162">Lämna förslag på funktioner i [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span><span class="sxs-lookup"><span data-stu-id="3f036-162">Give us feature suggestions on [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span></span>

## <a name="view-hello-solution"></a><span data-ttu-id="3f036-163">Visa hello lösning</span><span class="sxs-lookup"><span data-stu-id="3f036-163">View hello solution</span></span>

<span data-ttu-id="3f036-164">Det här avsnittet hjälper dig att hello lösning Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="3f036-164">This section guides you through hello solution UI.</span></span>

### <a name="predictive-maintenance-dashboard"></a><span data-ttu-id="3f036-165">Instrumentpanel för förebyggande underhåll</span><span class="sxs-lookup"><span data-stu-id="3f036-165">Predictive Maintenance Dashboard</span></span>

<span data-ttu-id="3f036-166">Den här sidan i hello webbprogram använder PowerBI JavaScript-kontroller (se hello [PowerBI-visuell information databasen][lnk-powerbi]) toovisualize:</span><span class="sxs-lookup"><span data-stu-id="3f036-166">This page in hello web application uses PowerBI JavaScript controls (see hello [PowerBI-visuals repository][lnk-powerbi]) toovisualize:</span></span>

* <span data-ttu-id="3f036-167">hello utdata från hello Stream Analytics-jobb i blob storage.</span><span class="sxs-lookup"><span data-stu-id="3f036-167">hello output data from hello Stream Analytics jobs in blob storage.</span></span>
* <span data-ttu-id="3f036-168">Hej RUL och cykel antal per flygplan motorn.</span><span class="sxs-lookup"><span data-stu-id="3f036-168">hello RUL and cycle count per aircraft engine.</span></span>

### <a name="observing-hello-behavior-of-hello-cloud-solution"></a><span data-ttu-id="3f036-169">Sett hello funktionssätt hello molnlösning</span><span class="sxs-lookup"><span data-stu-id="3f036-169">Observing hello behavior of hello cloud solution</span></span>

<span data-ttu-id="3f036-170">I hello Azure portal, navigerar toohello resursgrupp med hello lösningens namn du har valt tooview etablerade resurserna.</span><span class="sxs-lookup"><span data-stu-id="3f036-170">In hello Azure portal, navigate toohello resource group with hello solution name you chose tooview your provisioned resources.</span></span>

![][img-resource-group]

<span data-ttu-id="3f036-171">När du etablerar hello förkonfigurerade lösningen kan du få ett e-postmeddelande med en länk toohello Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="3f036-171">When you provision hello preconfigured solution, you receive an email with a link toohello Machine Learning workspace.</span></span> <span data-ttu-id="3f036-172">Du kan också navigera toohello Machine Learning-arbetsytan från hello [azureiotsuite.com] [ lnk-azureiotsuite] sidan för etablerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="3f036-172">You can also navigate toohello Machine Learning workspace from hello [azureiotsuite.com][lnk-azureiotsuite] page for your provisioned solution.</span></span> <span data-ttu-id="3f036-173">En panel är tillgänglig på den här sidan när hello lösningen är i hello **klar** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3f036-173">A tile is available on this page when hello solution is in hello **Ready** state.</span></span>

![][img-machine-learning]

<span data-ttu-id="3f036-174">Du kan se hello urvalet har etablerats med fyra simulerade enheter toorepresent två flygplan med två motorer per flygplan, var och en med fyra sensorer i hello lösning-portalen.</span><span class="sxs-lookup"><span data-stu-id="3f036-174">In hello solution portal, you can see that hello sample is provisioned with four simulated devices toorepresent two aircraft with two engines per aircraft, each with four sensors.</span></span> <span data-ttu-id="3f036-175">När du först navigerar toohello lösning portal har hello simulering stoppats.</span><span class="sxs-lookup"><span data-stu-id="3f036-175">When you first navigate toohello solution portal, hello simulation is stopped.</span></span>

![][img-simulation-stopped]

<span data-ttu-id="3f036-176">Klicka på **starta simuleringen** toobegin hello simuleringen.</span><span class="sxs-lookup"><span data-stu-id="3f036-176">Click **Start simulation** toobegin hello simulation.</span></span> <span data-ttu-id="3f036-177">Hej sensor historik, RUL, cykler och RUL historik fylla hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f036-177">hello sensor history, RUL, Cycles, and RUL history populate hello dashboard.</span></span>

![][img-simulation-running]

<span data-ttu-id="3f036-178">När RUL är mindre än 160 (en godtycklig tröskel valt), visas hello lösning en varning symbolen nästa toohello RUL visas.</span><span class="sxs-lookup"><span data-stu-id="3f036-178">When RUL is less than 160 (an arbitrary threshold chosen for demonstration purposes), hello solution portal displays a warning symbol next toohello RUL display.</span></span> <span data-ttu-id="3f036-179">hello lösning portal visar också hello flygplan motorn i gult.</span><span class="sxs-lookup"><span data-stu-id="3f036-179">hello solution portal also highlights hello aircraft engine in yellow.</span></span> <span data-ttu-id="3f036-180">Observera hur hello RUL värden ha en allmän nedåtgående trend övergripande men tenderar toobounce uppåt och nedåt.</span><span class="sxs-lookup"><span data-stu-id="3f036-180">Notice how hello RUL values have a general downward trend overall, but tend toobounce up and down.</span></span> <span data-ttu-id="3f036-181">Det här beteendet fås hello varierande cykel längder och hello modellen noggrannhet.</span><span class="sxs-lookup"><span data-stu-id="3f036-181">This behavior results from hello varying cycle lengths and hello model accuracy.</span></span>

![][img-simulation-warning]

<span data-ttu-id="3f036-182">hello fullständig simuleringen tar cirka 35 minuter toocomplete 148 cykler.</span><span class="sxs-lookup"><span data-stu-id="3f036-182">hello full simulation takes around 35 minutes toocomplete 148 cycles.</span></span> <span data-ttu-id="3f036-183">hello 160 RUL tröskelvärdet har uppnåtts för hello första gången vid cirka 5 minuter och båda motorer träffar hello tröskelvärdet cirka åtta minuter.</span><span class="sxs-lookup"><span data-stu-id="3f036-183">hello 160 RUL threshold is met for hello first time at around 5 minutes and both engines hit hello threshold at around 8 minutes.</span></span>

<span data-ttu-id="3f036-184">hello simuleringen körs via hello fullständiga datauppsättningen för 148 cykler och reglerar på slutvärdena RUL och cykel.</span><span class="sxs-lookup"><span data-stu-id="3f036-184">hello simulation runs through hello complete dataset for 148 cycles and settles on final RUL and cycle values.</span></span>

<span data-ttu-id="3f036-185">Du kan stoppa hello simuleringen vid någon plats, men att klicka på **starta simuleringen** repetitioner hello simulering från hello start av hello dataset.</span><span class="sxs-lookup"><span data-stu-id="3f036-185">You can stop hello simulation at any point, but clicking **Start Simulation** replays hello simulation from hello start of hello dataset.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f036-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3f036-186">Next steps</span></span>

<span data-ttu-id="3f036-187">Mer information om hur Azure IoT möjliggör scenarier med förutsägande Underhåll läsa toolearn [avbilda värde från hello Sakernas Internet][lnk_capture_value].</span><span class="sxs-lookup"><span data-stu-id="3f036-187">toolearn more about how Azure IoT enables predictive maintenance scenarios, read [Capture value from hello Internet of Things][lnk_capture_value].</span></span>

<span data-ttu-id="3f036-188">Ta en [genomgången] [ lnk-predictive-walkthrough] hello förutsägande Underhåll lösning.</span><span class="sxs-lookup"><span data-stu-id="3f036-188">Take a [walkthrough][lnk-predictive-walkthrough] of hello predictive maintenance solution.</span></span>

<span data-ttu-id="3f036-189">Du kan även utforska några av hello andra funktioner och förmågor i hello IoT Suite förkonfigurerade lösningar:</span><span class="sxs-lookup"><span data-stu-id="3f036-189">You can also explore some of hello other features and capabilities of hello IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="3f036-190">[Vanliga frågor och svar om IoT Suite][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="3f036-190">[Frequently asked questions for IoT Suite][lnk-faq]</span></span>
* <span data-ttu-id="3f036-191">[IoT-säkerhet från hello bakgrund][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="3f036-191">[IoT security from hello ground up][lnk-security-groundup]</span></span>

[img-resource-group]: media/iot-suite-predictive-overview/resource-group.png
[img-simulation-stopped]: media/iot-suite-predictive-overview/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-overview/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-overview/simulation-warning.png
[img-machine-learning]: media/iot-suite-predictive-overview/machine-learning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3

[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-permissions]: iot-suite-permissions.md
[lnk-portal]: http://portal.azure.com/
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/