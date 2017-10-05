---
title: "Förkonfigurerad lösning för förebyggande underhåll | Microsoft Docs"
description: "En beskrivning av den förkonfigurerade lösningen för förebyggande underhåll i Azure IoT Suite."
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
ms.openlocfilehash: 8bad198488c4940a83eb32ec02122a91d47ca86c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="predictive-maintenance-preconfigured-solution-overview"></a><span data-ttu-id="2d983-103">Översikt över förkonfigurerad lösning för förebyggande underhåll</span><span class="sxs-lookup"><span data-stu-id="2d983-103">Predictive maintenance preconfigured solution overview</span></span>

<span data-ttu-id="2d983-104">Den [förkonfigurerade lösningen][lnk_preconfigured_solutions] för *förebyggande underhåll* är en av de förkonfigurerade lösningarna i [Microsoft Azure IoT Suite][lnk_iot_suite].</span><span class="sxs-lookup"><span data-stu-id="2d983-104">The *predictive maintenance* [preconfigured solution][lnk_preconfigured_solutions] is one of the [Microsoft Azure IoT Suite][lnk_iot_suite] preconfigured solutions.</span></span> <span data-ttu-id="2d983-105">Den här lösningen integrerar realtidsinsamling av enhetstelemetri med en förebyggande modell skapad med [Azure Machine Learning][lnk-machine-learning].</span><span class="sxs-lookup"><span data-stu-id="2d983-105">This solution integrates real-time device telemetry collection with a predictive model created using [Azure Machine Learning][lnk-machine-learning].</span></span>

<span data-ttu-id="2d983-106">Med Azure IoT Suite kan du snabbt och enkelt ansluta till och övervaka tillgångar och analysera telemetri i realtid på instrumentpaneler och i visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="2d983-106">With Azure IoT Suite, you can quickly and easily connect to and monitor assets, and analyze telemetry in real time in dashboards and visualizations.</span></span> <span data-ttu-id="2d983-107">I lösningen för förutsägande underhåll ger instrumentpanelerna och visualiseringarna nya insikter som kan öka effektiviteten och förbättra intäktsströmmarna.</span><span class="sxs-lookup"><span data-stu-id="2d983-107">In the predictive maintenance solution, the dashboards and visualizations provide you with new intelligence that can drive efficiencies and enhance revenue streams.</span></span>

## <a name="the-scenario"></a><span data-ttu-id="2d983-108">Scenariot</span><span class="sxs-lookup"><span data-stu-id="2d983-108">The Scenario</span></span>

<span data-ttu-id="2d983-109">Fabrikam är ett regionalt flygbolag som fokuserar på bra kundupplevelser till konkurrenskraftiga priser.</span><span class="sxs-lookup"><span data-stu-id="2d983-109">Fabrikam is a regional airline that focuses on great customer experience at competitive prices.</span></span> <span data-ttu-id="2d983-110">En orsak till flygförseningar är underhållsproblem, och flygplansmotorernas underhåll är särskilt krävande.</span><span class="sxs-lookup"><span data-stu-id="2d983-110">One cause of flight delays is maintenance issues and aircraft engine maintenance is particularly challenging.</span></span> <span data-ttu-id="2d983-111">Fabrikam måste till vilket pris som helst förhindra motorfel under flygningar och inspekterar därför regelbundet sina motorer och schemalägger underhåll med utgångspunkt i en plan.</span><span class="sxs-lookup"><span data-stu-id="2d983-111">Fabrikam must avoid engine failure during flight at all costs, so it inspects its engines regularly and schedules maintenance according to a plan.</span></span> <span data-ttu-id="2d983-112">Flygplansmotorer slits dock inte alltid likadant.</span><span class="sxs-lookup"><span data-stu-id="2d983-112">However, aircraft engines don’t always wear the same.</span></span> <span data-ttu-id="2d983-113">En del onödigt underhåll utförs på motorerna.</span><span class="sxs-lookup"><span data-stu-id="2d983-113">Some unnecessary maintenance is performed on engines.</span></span> <span data-ttu-id="2d983-114">Dessutom kan det uppstå problem som gör att planet blir stående tills underhåll har utförts.</span><span class="sxs-lookup"><span data-stu-id="2d983-114">More importantly, issues arise which can ground an aircraft until maintenance is performed.</span></span> <span data-ttu-id="2d983-115">Om ett flygplan finns på en plats där rätt tekniker eller reservdelar inte är tillgängliga kan dessa problem bli extra kostsamma.</span><span class="sxs-lookup"><span data-stu-id="2d983-115">If an aircraft is at a location where the right technicians or spare parts are not available, these issues can be especially costly.</span></span>

<span data-ttu-id="2d983-116">Motorerna i Fabrikams flygplan är utrustade med sensorer som övervakar motortillståndet under flygning.</span><span class="sxs-lookup"><span data-stu-id="2d983-116">The engines of Fabrikam’s aircraft are instrumented with sensors that monitor engine conditions during flight.</span></span> <span data-ttu-id="2d983-117">Fabrikam använder lösningen för förutsägande underhåll för att samla in de sensordata som inhämtats under flygresan.</span><span class="sxs-lookup"><span data-stu-id="2d983-117">Fabrikam uses the predictive maintenance solution to collect the sensor data collected during the flight.</span></span> <span data-ttu-id="2d983-118">Fabrikams dataspecialister har under flera år samlat in data om motorernas drift och eventuella fel och har tagit fram en modell som kan förutse flygplansmotorernas återstående livslängd.</span><span class="sxs-lookup"><span data-stu-id="2d983-118">After accumulating years of engine operational and failure data, Fabrikam’s data scientists have modeled a way to predict the Remaining Useful Life (RUL) of an aircraft engine.</span></span> <span data-ttu-id="2d983-119">Modellen använder en korrelation mellan data från fyra av motorsensorerna och motorförslitningar som med tiden leder till motorfel.</span><span class="sxs-lookup"><span data-stu-id="2d983-119">The model uses a correlation between data from four of the engine sensors and engine wear that leads to eventual failure.</span></span> <span data-ttu-id="2d983-120">Fabrikam fortsätter att utföra regelbundna inspektioner för att säkerställa säkerheten, men nu kan man använda modeller för att beräkna varje motors återstående livslängd efter varje flygning.</span><span class="sxs-lookup"><span data-stu-id="2d983-120">While Fabrikam continues to perform regular inspections to ensure safety, it can now use the models to compute the RUL for each engine after every flight.</span></span> <span data-ttu-id="2d983-121">Modellen använder telemetrin som samlats in från motorerna under flygresan.</span><span class="sxs-lookup"><span data-stu-id="2d983-121">The model uses the telemetry collected from the engines during the flight.</span></span> <span data-ttu-id="2d983-122">Fabrikam kan nu förutsäga framtida tidpunkter för potentiella fel och planera för underhåll och reparation i förväg.</span><span class="sxs-lookup"><span data-stu-id="2d983-122">Fabrikam can now predict future points of failure and plan for maintenance and repair in advance.</span></span>

> [!NOTE]
> <span data-ttu-id="2d983-123">Lösningsmodellen använder faktiska data om motorernas slitage.</span><span class="sxs-lookup"><span data-stu-id="2d983-123">The solution model uses actual engine wear data.</span></span>

<span data-ttu-id="2d983-124">Genom att förutsäga när underhåll krävs kan Fabrikam optimera verksamheten och minska kostnaderna.</span><span class="sxs-lookup"><span data-stu-id="2d983-124">By predicting the point when maintenance is required, Fabrikam can optimize its operations to reduce costs.</span></span>

<span data-ttu-id="2d983-125">Underhållskoordinatorer arbetar tillsammans med schemaläggare för att:</span><span class="sxs-lookup"><span data-stu-id="2d983-125">Maintenance coordinators work with schedulers to:</span></span>

- <span data-ttu-id="2d983-126">Planera underhållet så att det sammanträffar med tidpunkten då ett flygplan stannar på en viss plats.</span><span class="sxs-lookup"><span data-stu-id="2d983-126">Plan maintenance to coincide with an aircraft stopping at a particular location.</span></span>
- <span data-ttu-id="2d983-127">Se till att det finns tillräckligt med tid för flygplanet att vara ur funktion utan att orsaka avbrott i schemat.</span><span class="sxs-lookup"><span data-stu-id="2d983-127">Ensure sufficient time is available for the aircraft to be out of service without causing schedule disruption.</span></span>
- <span data-ttu-id="2d983-128">För att schemalägga tekniker så att flygplanen servas effektivt utan väntetid.</span><span class="sxs-lookup"><span data-stu-id="2d983-128">To schedule technicians to ensure that aircraft are serviced efficiently without wait time.</span></span>

<span data-ttu-id="2d983-129">Inventeringskontrollchefer får underhållsplaner så att de kan optimera beställningsprocessen och reservdelslagret.</span><span class="sxs-lookup"><span data-stu-id="2d983-129">Inventory control managers receive maintenance plans, so they can optimize their ordering process and spare parts inventory.</span></span>

<span data-ttu-id="2d983-130">Dessa aktiviteter gör att Fabrikam kan minimera flygplanens marktid och minska driftkostnaderna samtidigt som man säkerställer passagerarnas och flygplansbesättningens säkerhet.</span><span class="sxs-lookup"><span data-stu-id="2d983-130">These activities enable Fabrikam to minimize aircraft ground time and reduce operating costs while ensuring the safety of passengers and crew.</span></span>

<span data-ttu-id="2d983-131">Mer information om hur [Azure IoT Suite][lnk_iot_suite] tillhandahåller de funktioner som kunder behöver för att förverkliga potentialen i förebyggande underhåll finns [här][lnk_infographic].</span><span class="sxs-lookup"><span data-stu-id="2d983-131">To understand how [Azure IoT Suite][lnk_iot_suite] provides the capabilities customers need to realize the potential of predictive maintenance, review this [infographic][lnk_infographic].</span></span>

## <a name="how-the-predictive-maintenance-solution-is-built"></a><span data-ttu-id="2d983-132">Så här är lösningen för förebyggande underhåll uppbyggd</span><span class="sxs-lookup"><span data-stu-id="2d983-132">How the predictive maintenance solution is built</span></span>

<span data-ttu-id="2d983-133">Lösningen utnyttjar en befintlig Azure Machine Learning-modell som är tillgänglig som en mall för att demonstrera hur dessa funktioner arbetar med enhetstelemetri som samlas in via IoT Suite-tjänster.</span><span class="sxs-lookup"><span data-stu-id="2d983-133">The solution uses an existing Azure Machine Learning model available as a template to show these capabilities working from device telemetry collected through IoT Suite services.</span></span> <span data-ttu-id="2d983-134">Microsoft har byggt en [regressionsmodell][lnk_regression_model] av en flygplansmotor baserat på offentligt tillgängliga data<sup>\[1\]</sup> och stegvisa anvisningar för hur du använder modellen.</span><span class="sxs-lookup"><span data-stu-id="2d983-134">Microsoft has built a [regression model][lnk_regression_model] of an aircraft engine based on publically available data<sup>\[1\]</sup>, and step-by-step guidance on how to use the model.</span></span>

<span data-ttu-id="2d983-135">Azure IoT:s lösning för förutsägande underhåll använder regressionsmodellen som skapats från den här mallen.</span><span class="sxs-lookup"><span data-stu-id="2d983-135">The Azure IoT predictive maintenance solution uses the regression model created from this template.</span></span> <span data-ttu-id="2d983-136">Modellen distribueras i din Azure-prenumeration och exponeras via en automatiskt genererad API.</span><span class="sxs-lookup"><span data-stu-id="2d983-136">The model is deployed into your Azure subscription and exposed through an automatically generated API.</span></span> <span data-ttu-id="2d983-137">Lösningen innehåller en delmängd av testdata som representerar 4 (av totalt 100) motorer och 4 (av totalt 21) dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="2d983-137">The solution includes a subset of the testing data representing 4 (of 100 total) engines and the 4 (of 21 total) sensor data streams.</span></span> <span data-ttu-id="2d983-138">Dessa data är tillräckliga för att tillhandahålla ett korrekt resultat från Trained Model.</span><span class="sxs-lookup"><span data-stu-id="2d983-138">This data is sufficient to provide an accurate result from the trained model.</span></span>

<span data-ttu-id="2d983-139">*\[1\] A. Saxena och K. Goebel (2008). "Turbofan Engine Degradation Simulation Data Set", NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*</span><span class="sxs-lookup"><span data-stu-id="2d983-139">*\[1\] A. Saxena and K. Goebel (2008). "Turbofan Engine Degradation Simulation Data Set", NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*</span></span>

## <a name="get-started-with-predictive-maintenance"></a><span data-ttu-id="2d983-140">Kom igång med förebyggande underhåll</span><span class="sxs-lookup"><span data-stu-id="2d983-140">Get started with predictive maintenance</span></span>

<span data-ttu-id="2d983-141">I den här självstudien får du se hur du kan etablera lösningen för förebyggande underhåll.</span><span class="sxs-lookup"><span data-stu-id="2d983-141">This tutorial shows you how to provision the predictive maintenance solution.</span></span> <span data-ttu-id="2d983-142">Du lär dig också om de grundläggande funktionerna i lösningen för förebyggande underhåll.</span><span class="sxs-lookup"><span data-stu-id="2d983-142">It also walks you through the basic features of the predictive maintenance solution.</span></span> <span data-ttu-id="2d983-143">Du kan komma åt många av dessa funktioner via instrumentpanelen för lösningen som distribueras tillsammans med den förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="2d983-143">You can access many of these features through the solution dashboard that deploys along with the preconfigured solution.</span></span>

<span data-ttu-id="2d983-144">Du behöver en aktiv Azure-prenumeration för att kunna utföra stegen i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="2d983-144">To complete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="2d983-145">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="2d983-145">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="2d983-146">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="2d983-146">For details, see [Azure Free Trial][lnk_free_trial].</span></span>

1. <span data-ttu-id="2d983-147">Logga in på [azureiotsuite.com][lnk-azureiotsuite] med din Azure-kontoinformation och skapa en lösning genom att klicka på **+**.</span><span class="sxs-lookup"><span data-stu-id="2d983-147">Log on to [azureiotsuite.com][lnk-azureiotsuite] using your Azure account credentials, and click **+** to create a solution.</span></span>
1. <span data-ttu-id="2d983-148">Klicka på **Välj** på ikonen **Förebyggande underhåll**.</span><span class="sxs-lookup"><span data-stu-id="2d983-148">Click **Select** the **Predictive maintenance** tile.</span></span>
1. <span data-ttu-id="2d983-149">Ange ett **lösningsnamn** för din förkonfigurerade lösning för förebyggande underhåll.</span><span class="sxs-lookup"><span data-stu-id="2d983-149">Enter a **Solution name** for your predictive maintenance preconfigured solution.</span></span>
1. <span data-ttu-id="2d983-150">Välj den **region** och **prenumeration** som du vill använda för att etablera lösningen.</span><span class="sxs-lookup"><span data-stu-id="2d983-150">Select the **Region** and **Subscription** you want to use to provision the solution.</span></span>
1. <span data-ttu-id="2d983-151">Klicka på **Skapa lösning** för att påbörja etableringen.</span><span class="sxs-lookup"><span data-stu-id="2d983-151">Click **Create Solution** to begin the provisioning process.</span></span> <span data-ttu-id="2d983-152">Den här processen tar normalt flera minuter.</span><span class="sxs-lookup"><span data-stu-id="2d983-152">This process typically takes several minutes to run.</span></span>

### <a name="wait-for-the-provisioning-process-to-complete"></a><span data-ttu-id="2d983-153">Vänta tills etableringsprocessen har slutförts</span><span class="sxs-lookup"><span data-stu-id="2d983-153">Wait for the provisioning process to complete</span></span>

1. <span data-ttu-id="2d983-154">Klicka på ikonen för din lösning med statusen **Etablerar**.</span><span class="sxs-lookup"><span data-stu-id="2d983-154">Click the tile for your solution with **Provisioning** status.</span></span>
1. <span data-ttu-id="2d983-155">Observera **etableringsstatusen** när Azure-tjänsterna distribueras i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2d983-155">Notice the **Provisioning states** as Azure services are deployed in your Azure subscription.</span></span>
1. <span data-ttu-id="2d983-156">När etableringen har slutförts ändras statusen till **Klar**.</span><span class="sxs-lookup"><span data-stu-id="2d983-156">Once provisioning completes, the status changes to **Ready**.</span></span>
1. <span data-ttu-id="2d983-157">Klicka på ikonen så ser du informationen om din lösning i den högra rutan.</span><span class="sxs-lookup"><span data-stu-id="2d983-157">Click the tile to see the details of your solution in the right-hand pane.</span></span> <span data-ttu-id="2d983-158">Från det här fönstret kan du starta lösningens instrumentpanel och få åtkomst till Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="2d983-158">From this pane, you can launch the solution dashboard and access the Machine Learning workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="2d983-159">Om det uppstår några problem när du distribuerar den förkonfigurerade lösningen kan du läsa [Behörigheter på webbplatsen azureiotsuite.com][lnk-permissions] och [Vanliga frågor och svar][lnk-faq].</span><span class="sxs-lookup"><span data-stu-id="2d983-159">If you encounter issues deploying the preconfigured solution, review [Permissions on the azureiotsuite.com site][lnk-permissions] and the [FAQ][lnk-faq].</span></span> <span data-ttu-id="2d983-160">Om problemen kvarstår så skapa en tjänstbiljett på [portalen][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="2d983-160">If the issues persist, create a service ticket on the [portal][lnk-portal].</span></span>

<span data-ttu-id="2d983-161">Finns det något som du förväntar dig att se men som inte visas för din lösning?</span><span class="sxs-lookup"><span data-stu-id="2d983-161">Are there details you'd expect to see that aren't listed for your solution?</span></span> <span data-ttu-id="2d983-162">Lämna förslag på funktioner i [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span><span class="sxs-lookup"><span data-stu-id="2d983-162">Give us feature suggestions on [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span></span>

## <a name="view-the-solution"></a><span data-ttu-id="2d983-163">Visa lösningen</span><span class="sxs-lookup"><span data-stu-id="2d983-163">View the solution</span></span>

<span data-ttu-id="2d983-164">I det här avsnittet vägleds du genom lösningens användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="2d983-164">This section guides you through the solution UI.</span></span>

### <a name="predictive-maintenance-dashboard"></a><span data-ttu-id="2d983-165">Instrumentpanel för förebyggande underhåll</span><span class="sxs-lookup"><span data-stu-id="2d983-165">Predictive Maintenance Dashboard</span></span>

<span data-ttu-id="2d983-166">Den här sidan i webbappen använder PowerBI JavaScript-kontroller (finns i [PowerBI-visuals-databasen][lnk-powerbi]) för att visualisera:</span><span class="sxs-lookup"><span data-stu-id="2d983-166">This page in the web application uses PowerBI JavaScript controls (see the [PowerBI-visuals repository][lnk-powerbi]) to visualize:</span></span>

* <span data-ttu-id="2d983-167">Utdata från Stream Analytics-jobben i Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="2d983-167">The output data from the Stream Analytics jobs in blob storage.</span></span>
* <span data-ttu-id="2d983-168">Den återstående användbara livslängden och antalet cykler per flygplansmotor.</span><span class="sxs-lookup"><span data-stu-id="2d983-168">The RUL and cycle count per aircraft engine.</span></span>

### <a name="observing-the-behavior-of-the-cloud-solution"></a><span data-ttu-id="2d983-169">Observera molnlösningens beteende</span><span class="sxs-lookup"><span data-stu-id="2d983-169">Observing the behavior of the cloud solution</span></span>

<span data-ttu-id="2d983-170">I Azure Portal navigerar du till resursgruppen med det lösningsnamn som du har valt för att visa dina etablerade resurser.</span><span class="sxs-lookup"><span data-stu-id="2d983-170">In the Azure portal, navigate to the resource group with the solution name you chose to view your provisioned resources.</span></span>

![][img-resource-group]

<span data-ttu-id="2d983-171">När du etablerar den förkonfigurerade lösningen kan du få ett e-postmeddelande med en länk till Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="2d983-171">When you provision the preconfigured solution, you receive an email with a link to the Machine Learning workspace.</span></span> <span data-ttu-id="2d983-172">Du kan också navigera till Machine Learning-arbetsytan från sidan [azureiotsuite.com][lnk-azureiotsuite] för din etablerade lösning.</span><span class="sxs-lookup"><span data-stu-id="2d983-172">You can also navigate to the Machine Learning workspace from the [azureiotsuite.com][lnk-azureiotsuite] page for your provisioned solution.</span></span> <span data-ttu-id="2d983-173">En panel är tillgänglig på den här sidan när lösningen har statusen **Redo**.</span><span class="sxs-lookup"><span data-stu-id="2d983-173">A tile is available on this page when the solution is in the **Ready** state.</span></span>

![][img-machine-learning]

<span data-ttu-id="2d983-174">På lösningsportalen kan du se att exemplet etableras med fyra simulerade enheter som representerar två flygplan med två motorer per plan, var och en med fyra sensorer.</span><span class="sxs-lookup"><span data-stu-id="2d983-174">In the solution portal, you can see that the sample is provisioned with four simulated devices to represent two aircraft with two engines per aircraft, each with four sensors.</span></span> <span data-ttu-id="2d983-175">När du navigerar till lösningsportalen stoppas simuleringen.</span><span class="sxs-lookup"><span data-stu-id="2d983-175">When you first navigate to the solution portal, the simulation is stopped.</span></span>

![][img-simulation-stopped]

<span data-ttu-id="2d983-176">Starta simuleringen genom att klicka på **Starta simulering**.</span><span class="sxs-lookup"><span data-stu-id="2d983-176">Click **Start simulation** to begin the simulation.</span></span> <span data-ttu-id="2d983-177">Instrumentpanelen fylls med sensorhistorik, RUL-värden, cykler och RUL-historik.</span><span class="sxs-lookup"><span data-stu-id="2d983-177">The sensor history, RUL, Cycles, and RUL history populate the dashboard.</span></span>

![][img-simulation-running]

<span data-ttu-id="2d983-178">Om RUL-värdet är mindre än 160 (ett godtyckligt tröskelvärde som valts som exempel) visas en varningssymbol på lösningsportalen bredvid RUL-värdet.</span><span class="sxs-lookup"><span data-stu-id="2d983-178">When RUL is less than 160 (an arbitrary threshold chosen for demonstration purposes), the solution portal displays a warning symbol next to the RUL display.</span></span> <span data-ttu-id="2d983-179">Dessutom markeras flygplansmotorn i gult på lösningsportalen.</span><span class="sxs-lookup"><span data-stu-id="2d983-179">The solution portal also highlights the aircraft engine in yellow.</span></span> <span data-ttu-id="2d983-180">Lägg märke till att RUL-värdena har en nedåtgående trend generellt, men med många upp- och nedgångar.</span><span class="sxs-lookup"><span data-stu-id="2d983-180">Notice how the RUL values have a general downward trend overall, but tend to bounce up and down.</span></span> <span data-ttu-id="2d983-181">Detta mönster beror på de olika cykellängderna och modellens precision.</span><span class="sxs-lookup"><span data-stu-id="2d983-181">This behavior results from the varying cycle lengths and the model accuracy.</span></span>

![][img-simulation-warning]

<span data-ttu-id="2d983-182">Den fullständiga simuleringen tar 35 minuter för att slutföra 148 cykler.</span><span class="sxs-lookup"><span data-stu-id="2d983-182">The full simulation takes around 35 minutes to complete 148 cycles.</span></span> <span data-ttu-id="2d983-183">RUL-tröskelvärdet på 160 nås för första gången vid cirka 5 minuter och båda motorerna når tröskelvärdet ungefär vid 8 minuter.</span><span class="sxs-lookup"><span data-stu-id="2d983-183">The 160 RUL threshold is met for the first time at around 5 minutes and both engines hit the threshold at around 8 minutes.</span></span>

<span data-ttu-id="2d983-184">Simuleringen kör igenom den fullständiga datauppsättningen i 148 cykler och bestämmer den slutliga återstående användbara livslängden och cykelvärdena.</span><span class="sxs-lookup"><span data-stu-id="2d983-184">The simulation runs through the complete dataset for 148 cycles and settles on final RUL and cycle values.</span></span>

<span data-ttu-id="2d983-185">Du kan stoppa simuleringen när du vill, men om du klickar på **Starta simulering** spelas simuleringen upp igen från början av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="2d983-185">You can stop the simulation at any point, but clicking **Start Simulation** replays the simulation from the start of the dataset.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d983-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2d983-186">Next steps</span></span>

<span data-ttu-id="2d983-187">Mer information om hur Azure IoT möjliggör scenarier för förebyggande underhåll finns i artikeln [Få ut värde av Internet of Things][lnk_capture_value].</span><span class="sxs-lookup"><span data-stu-id="2d983-187">To learn more about how Azure IoT enables predictive maintenance scenarios, read [Capture value from the Internet of Things][lnk_capture_value].</span></span>

<span data-ttu-id="2d983-188">Gå en [genomgång][lnk-predictive-walkthrough] av lösningen för förutsägande underhåll.</span><span class="sxs-lookup"><span data-stu-id="2d983-188">Take a [walkthrough][lnk-predictive-walkthrough] of the predictive maintenance solution.</span></span>

<span data-ttu-id="2d983-189">Du kan även utforska några andra funktioner och möjligheter i de förkonfigurerade lösningarna i IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="2d983-189">You can also explore some of the other features and capabilities of the IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="2d983-190">[Vanliga frågor och svar om IoT Suite][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="2d983-190">[Frequently asked questions for IoT Suite][lnk-faq]</span></span>
* <span data-ttu-id="2d983-191">[IoT-säkerhet från grunden][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="2d983-191">[IoT security from the ground up][lnk-security-groundup]</span></span>

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