---
title: "aaaPower BI-instrumentpanel för vehicle hälso- och köra vanor - Azure | Microsoft Docs"
description: "Använda hello funktionerna i Cortana Intelligence toogain i realtid och förutsägbara insikter på vehicle hälsotillstånd och andra vanor."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: bradsev
ms.openlocfilehash: 0bd054d943387ecad7301236eebae22458173aba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a><span data-ttu-id="58037-103">Vehicle telemetri analytics lösning mallen Power BI-instrumentpanel instruktioner</span><span class="sxs-lookup"><span data-stu-id="58037-103">Vehicle telemetry analytics solution template Power BI Dashboard setup instructions</span></span>
<span data-ttu-id="58037-104">Detta **menyn** länkar toohello kapitlen i den här playbook.</span><span class="sxs-lookup"><span data-stu-id="58037-104">This **menu** links toohello chapters in this playbook.</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

<span data-ttu-id="58037-105">hello Vehicle telemetri Analytics lösningen visar hur bil hos återförsäljarna, bil tillverkare och försäkringsbolag kan utnyttja hello funktionerna i Cortana Intelligence toogain i realtid och förutsägbara insikter om vehicle hälsa och körning vanor toodrive förbättringar i hello area kunden upplevelse, R & D och marknadsföringskampanjer.</span><span class="sxs-lookup"><span data-stu-id="58037-105">hello Vehicle Telemetry Analytics solution showcases how car dealerships, automobile manufacturers and insurance companies can leverage hello capabilities of Cortana Intelligence toogain real-time and predictive insights on vehicle health and driving habits toodrive improvements in hello area of customer experience, R&D and marketing campaigns.</span></span> <span data-ttu-id="58037-106">Det här dokumentet innehåller steg-för-steg-anvisningar om hur du kan konfigurera hello Power BI-rapporter och instrumentpanel när hello lösningen har distribuerats i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="58037-106">This document contains step by step instructions on how you can configure hello Power BI reports and dashboard once hello solution is deployed in your subscription.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="58037-107">Krav</span><span class="sxs-lookup"><span data-stu-id="58037-107">Prerequisites</span></span>
1. <span data-ttu-id="58037-108">Distribuera hello [telemetri Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) lösning</span><span class="sxs-lookup"><span data-stu-id="58037-108">Deploy hello [Telemetry Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) solution</span></span>  
2. [<span data-ttu-id="58037-109">Installera Microsoft Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="58037-109">Install Microsoft Power BI Desktop</span></span>](http://www.microsoft.com/download/details.aspx?id=45331)
3. <span data-ttu-id="58037-110">En [Azure-prenumeration](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="58037-110">An [Azure subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="58037-111">Om du inte har en Azure-prenumeration, komma igång med Azure kostnadsfri prenumeration</span><span class="sxs-lookup"><span data-stu-id="58037-111">If you don't have an Azure subscription, get started with Azure free subscription</span></span>
4. <span data-ttu-id="58037-112">Microsoft Power BI-konto</span><span class="sxs-lookup"><span data-stu-id="58037-112">Microsoft Power BI account</span></span>

## <a name="cortana-intelligence-suite-components"></a><span data-ttu-id="58037-113">Cortana Intelligence Suite-komponenter</span><span class="sxs-lookup"><span data-stu-id="58037-113">Cortana Intelligence Suite Components</span></span>
<span data-ttu-id="58037-114">Som en del av hello Vehicle telemetri Analytics lösningsmall distribueras hello följande Cortana Intelligence-tjänster i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="58037-114">As part of hello Vehicle Telemetry Analytics solution template, hello following Cortana Intelligence services are deployed in your subscription.</span></span>

* <span data-ttu-id="58037-115">**Event Hub** för att föra in miljontals vehicle telemetriska händelser i Azure.</span><span class="sxs-lookup"><span data-stu-id="58037-115">**Event Hub** for ingesting millions of vehicle telemetry events into Azure.</span></span>
* <span data-ttu-id="58037-116">**Strömma Analytics** för att få realtidsinsikter på vehicle hälsa och kvarstår dessa data till långsiktig lagring för bättre batch analytics.</span><span class="sxs-lookup"><span data-stu-id="58037-116">**Stream Analytics** for gaining real-time insights on vehicle health and persists that data into long-term storage for richer batch analytics.</span></span>
* <span data-ttu-id="58037-117">**Machine Learning** för identifiering av avvikelse i realtid och batchbearbetning toogain förutsägande insikter.</span><span class="sxs-lookup"><span data-stu-id="58037-117">**Machine Learning** for anomaly detection in real-time and batch processing toogain predictive insights.</span></span>
* <span data-ttu-id="58037-118">**HDInsight** är balanserad tootransform data i skala</span><span class="sxs-lookup"><span data-stu-id="58037-118">**HDInsight** is leveraged tootransform data at scale</span></span>
* <span data-ttu-id="58037-119">**Data Factory** hanterar orchestration, schemaläggning, resurshantering och övervakning av hello batch process-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="58037-119">**Data Factory** handles orchestration, scheduling, resource management and monitoring of hello batch processing pipeline.</span></span>

<span data-ttu-id="58037-120">**Power BI** ger den här lösningen en omfattande instrumentpanel för data i realtid och förutsägelseanalys visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="58037-120">**Power BI** gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span> 

<span data-ttu-id="58037-121">hello lösningen använder två olika datakällor: **simulerade vehicle signaler och diagnostik dataset** och **vehicle katalogen**.</span><span class="sxs-lookup"><span data-stu-id="58037-121">hello solution uses two different data sources: **Simulated vehicle signals and diagnostic dataset** and **vehicle catalog**.</span></span>

<span data-ttu-id="58037-122">Vehicle telematik simulator ingår som en del av den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="58037-122">A vehicle telematics simulator is included as part of this solution.</span></span> <span data-ttu-id="58037-123">Den genererar diagnostisk information och signalerar till motsvarande toohello tillstånd hello vehicle och intresseväckande mönster vid en viss tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="58037-123">It emits diagnostic information and signals corresponding toohello state of hello vehicle and driving pattern at a given point in time.</span></span> 

<span data-ttu-id="58037-124">hello Vehicle katalog är en referens dataset som innehåller VIN toomodel mappning</span><span class="sxs-lookup"><span data-stu-id="58037-124">hello Vehicle Catalog is a reference dataset containing VIN toomodel mapping</span></span>

## <a name="power-bi-dashboard-preparation"></a><span data-ttu-id="58037-125">Förberedelse av Power BI-instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="58037-125">Power BI Dashboard Preparation</span></span>
### <a name="setup-power-bi-real-time-dashboard"></a><span data-ttu-id="58037-126">Konfigurera realtid Power BI-instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="58037-126">Setup Power BI Real-Time Dashboard</span></span>

<span data-ttu-id="58037-127">**Starta hello realtid instrumentpanelen programmet** när hello distributionen är klar bör du följa hello manuell åtgärd instruktioner</span><span class="sxs-lookup"><span data-stu-id="58037-127">**Start hello real-time dashboard application** Once hello deployment is completed, you should follow hello Manual Operation Instructions</span></span>

* <span data-ttu-id="58037-128">Hämta realtid instrumentpanelen programmet RealtimeDashboardApp.zip och packa upp den.</span><span class="sxs-lookup"><span data-stu-id="58037-128">Download real-time dashboard application RealtimeDashboardApp.zip, and unzip it.</span></span>
*  <span data-ttu-id="58037-129">Öppna appen konfigurationsfilen RealtimeDashboardApp.exe.config, Ersätt appSettings för Eventhub, Blob Storage och ML service-anslutningar med hello värden i hello manuell åtgärd instruktioner och spara ändringarna i uppackade hello-mappen.</span><span class="sxs-lookup"><span data-stu-id="58037-129">In hello unzipped folder, open app config file 'RealtimeDashboardApp.exe.config', replace appSettings for Eventhub, Blob Storage, and ML service connections with hello values in hello Manual Operation Instructions, and save your changes.</span></span>
* <span data-ttu-id="58037-130">Kör program RealtimeDashboardApp.exe.</span><span class="sxs-lookup"><span data-stu-id="58037-130">Run application RealtimeDashboardApp.exe.</span></span> <span data-ttu-id="58037-131">Ett inloggningsfönster kommer popup-, ange din giltiga PowerBI-autentiseringsuppgifter och klickar på hello **acceptera** knappen.</span><span class="sxs-lookup"><span data-stu-id="58037-131">A login window will pop up, provide your valid PowerBI credentials and click hello **Accept** button.</span></span> <span data-ttu-id="58037-132">Hello app startar toorun.</span><span class="sxs-lookup"><span data-stu-id="58037-132">Then hello app will start toorun.</span></span>

   ![Logga in tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
   ![Power BI-instrumentpanel behörigheter](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

* <span data-ttu-id="58037-135">Inloggningen tooPowerBI webbplats och skapa instrumentpanel i realtid.</span><span class="sxs-lookup"><span data-stu-id="58037-135">Login tooPowerBI website, and create real-time dashboard.</span></span>

<span data-ttu-id="58037-136">Nu är du redo tooconfigure hello Powerbi-instrumentpanel med omfattande visualiseringar toogain realtid och förutsägbara insikter om vehicle hälsa och köra vanor.</span><span class="sxs-lookup"><span data-stu-id="58037-136">Now, you are ready tooconfigure hello Power BI dashboard with rich visualizations toogain real-time and predictive insights on vehicle health and driving habits.</span></span> <span data-ttu-id="58037-137">Det tar cirka 45 minuter tooan timme toocreate alla hello rapporter och konfigurera hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="58037-137">It takes about 45 minutes tooan hour toocreate all hello reports and configure hello dashboard.</span></span> 

### <a name="configure-power-bi-reports"></a><span data-ttu-id="58037-138">Konfigurera Power BI-rapporter</span><span class="sxs-lookup"><span data-stu-id="58037-138">Configure Power BI reports</span></span>
<span data-ttu-id="58037-139">ta om 30 45 minuter toocomplete hello realtid rapporter och hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="58037-139">hello real-time reports and hello dashboard take about 30-45 minutes toocomplete.</span></span> <span data-ttu-id="58037-140">Bläddra för[http://powerbi.com](http://powerbi.com) och logga in.</span><span class="sxs-lookup"><span data-stu-id="58037-140">Browse too[http://powerbi.com](http://powerbi.com) and login.</span></span>

![Logga in tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

<span data-ttu-id="58037-142">En ny datamängd skapas i Power BI.</span><span class="sxs-lookup"><span data-stu-id="58037-142">A new dataset is generated in Power BI.</span></span> <span data-ttu-id="58037-143">Klicka på hello **ConnectedCarsRealtime** dataset.</span><span class="sxs-lookup"><span data-stu-id="58037-143">Click hello **ConnectedCarsRealtime** dataset.</span></span>

![Markerad anslutna bilar realtid dataset](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

<span data-ttu-id="58037-145">Spara hello tom rapport med **Ctrl + s**.</span><span class="sxs-lookup"><span data-stu-id="58037-145">Save hello blank report using **Ctrl + s**.</span></span>

![Spara tom rapport](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

<span data-ttu-id="58037-147">Ange rapportens namn *Vehicle telemetri Analytics realtid - rapporter*.</span><span class="sxs-lookup"><span data-stu-id="58037-147">Provide report name *Vehicle Telemetry Analytics Real-time - Reports*.</span></span>

![Ange rapportens namn](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a><span data-ttu-id="58037-149">Realtid rapporter</span><span class="sxs-lookup"><span data-stu-id="58037-149">Real-time reports</span></span>
<span data-ttu-id="58037-150">Det finns tre realtid rapporter i den här lösningen:</span><span class="sxs-lookup"><span data-stu-id="58037-150">There are three real-time reports in this solution:</span></span>

1. <span data-ttu-id="58037-151">Fordon i åtgärden</span><span class="sxs-lookup"><span data-stu-id="58037-151">Vehicles in operation</span></span>
2. <span data-ttu-id="58037-152">Fordon kräver Underhåll</span><span class="sxs-lookup"><span data-stu-id="58037-152">Vehicles Requiring Maintenance</span></span>
3. <span data-ttu-id="58037-153">Fordon hälsostatistik</span><span class="sxs-lookup"><span data-stu-id="58037-153">Vehicles Health Statistics</span></span>

<span data-ttu-id="58037-154">Du kan välja tooconfigure alla hello tre realtid rapporter eller stoppa efter någon gång och fortsätta toohello nästa avsnitt för att konfigurera hello batch-rapporter.</span><span class="sxs-lookup"><span data-stu-id="58037-154">You can choose tooconfigure all hello three real-time reports or stop after any stage and proceed toohello next section of configuring hello batch reports.</span></span> <span data-ttu-id="58037-155">Vi rekommenderar att du toocreate alla hello tre rapporterar toovisualize hello fullständig insikter hello realtid sökväg hello lösning.</span><span class="sxs-lookup"><span data-stu-id="58037-155">We recommend you toocreate all hello three reports toovisualize hello full insights of hello real-time path of hello solution.</span></span>  

### <a name="1-vehicles-in-operation"></a><span data-ttu-id="58037-156">1. Fordon i åtgärden</span><span class="sxs-lookup"><span data-stu-id="58037-156">1. Vehicles in operation</span></span>
<span data-ttu-id="58037-157">Dubbelklicka på **sida 1** och Byt namn på den för ”fordon i åtgärden”</span><span class="sxs-lookup"><span data-stu-id="58037-157">Double-click **Page 1** and rename it too“Vehicles in operation”</span></span>  
    <span data-ttu-id="58037-158">![Anslutna bilar - fordon i åtgärd](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)</span><span class="sxs-lookup"><span data-stu-id="58037-158">![Connected Cars - Vehicles in operation](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)</span></span>  

<span data-ttu-id="58037-159">Välj **vin** från **fält** och välj typ av visualiseringen som **”kort”**.</span><span class="sxs-lookup"><span data-stu-id="58037-159">Select **vin** field from **Fields** and choose visualization type as **“Card”**.</span></span>  

<span data-ttu-id="58037-160">Kort visualiseringen skapas som visas i bild.</span><span class="sxs-lookup"><span data-stu-id="58037-160">Card visualization is created as shown in figure.</span></span>  
    ![Anslutna bilar - väljer vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

<span data-ttu-id="58037-162">Klicka på hello tomt område tooadd nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="58037-162">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="58037-163">Välj **Stad** och **vin** från fält.</span><span class="sxs-lookup"><span data-stu-id="58037-163">Select **City** and **vin** from fields.</span></span> <span data-ttu-id="58037-164">Ändra visualiseringen för**”karta”**.</span><span class="sxs-lookup"><span data-stu-id="58037-164">Change visualization too**“Map”**.</span></span> <span data-ttu-id="58037-165">Dra **vin** i värdeområdet.</span><span class="sxs-lookup"><span data-stu-id="58037-165">Drag **vin** in values area.</span></span> <span data-ttu-id="58037-166">Dra **Stad** från fält för**förklaring** område.</span><span class="sxs-lookup"><span data-stu-id="58037-166">Drag **city** from fields too**Legend** area.</span></span>   
    <span data-ttu-id="58037-167">![Ansluten bilar - kort visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)</span><span class="sxs-lookup"><span data-stu-id="58037-167">![Connected Cars - Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)</span></span>

<span data-ttu-id="58037-168">Välj **format** avsnittet från **visualiseringar**, klickar du på **rubrik** och ändra hello **Text** för**”fordon i åtgärden efter ort ”**.</span><span class="sxs-lookup"><span data-stu-id="58037-168">Select **format** section from **Visualizations**, click **Title** and change hello **Text** too**“Vehicles in operation by city”**.</span></span>  
    <span data-ttu-id="58037-169">![Anslutna bilar - fordon i åtgärd efter ort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)</span><span class="sxs-lookup"><span data-stu-id="58037-169">![Connected Cars - Vehicles in operation by city](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)</span></span>   

<span data-ttu-id="58037-170">Sista visualiseringen ser ut som visas i bild.</span><span class="sxs-lookup"><span data-stu-id="58037-170">Final visualization looks as shown in figure.</span></span>    
    ![Anslutna bilar - slutliga visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

<span data-ttu-id="58037-172">Klicka på hello tomt område tooadd nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="58037-172">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="58037-173">Välj **Stad** och **vin**, ändra typen av visualisering för**grupperat stående stapeldiagram**.</span><span class="sxs-lookup"><span data-stu-id="58037-173">Select **City** and **vin**, change visualization type too**Clustered Column Chart**.</span></span> <span data-ttu-id="58037-174">Se till att **Stad** i **axel området** och **vin** i **värdet område**</span><span class="sxs-lookup"><span data-stu-id="58037-174">Ensure **City** field in **Axis area** and **vin** in **Value area**</span></span>  

<span data-ttu-id="58037-175">Sortera diagram av **”antal vin”**</span><span class="sxs-lookup"><span data-stu-id="58037-175">Sort chart by **“Count of vin”**</span></span>  
    <span data-ttu-id="58037-176">![Anslutna bilar - antal vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)</span><span class="sxs-lookup"><span data-stu-id="58037-176">![Connected Cars - Count of vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)</span></span>  

<span data-ttu-id="58037-177">Ändra diagramtyp **rubrik** för**”fordon i åtgärd efter ort”**</span><span class="sxs-lookup"><span data-stu-id="58037-177">Change chart **Title** too**“Vehicles in operation by city”**</span></span>  

<span data-ttu-id="58037-178">Klicka på hello **Format** avsnittet och väljer sedan **Data färger**, klicka på hello **”på”** för**visa alla**</span><span class="sxs-lookup"><span data-stu-id="58037-178">Click hello **Format** section, then select **Data Colors**,  Click hello **“On”** too**Show All**</span></span>  
    <span data-ttu-id="58037-179">![Anslutna bilar – visa alla Data färger](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)</span><span class="sxs-lookup"><span data-stu-id="58037-179">![Connected Cars - Show all Data Colors](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)</span></span>  

<span data-ttu-id="58037-180">Ändra hello färg på enskilda ort genom att klicka på ikonen färg.</span><span class="sxs-lookup"><span data-stu-id="58037-180">Change hello color of individual city by clicking on color icon.</span></span>  
    ![Ansluten bilar - ändra färger](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

<span data-ttu-id="58037-182">Klicka på hello tomt område tooadd nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="58037-182">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="58037-183">Välj **grupperat stående stapeldiagram** visualiseringen från visualiseringar, dra **Stad** i **axel** området **modellen** i **förklaring** området och **vin** i **värdet** område.</span><span class="sxs-lookup"><span data-stu-id="58037-183">Select **Clustered Column Chart** visualization from visualizations, drag **city** field in **Axis** area, **Model** in **Legend** area and **vin** in **Value** area.</span></span>  
    <span data-ttu-id="58037-184">![Anslutna bilar - grupperat stående stapeldiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)</span><span class="sxs-lookup"><span data-stu-id="58037-184">![Connected Cars - Clustered Column Chart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)</span></span>  
    <span data-ttu-id="58037-185">![Anslutna bilar - återgivning](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)</span><span class="sxs-lookup"><span data-stu-id="58037-185">![Connected Cars - Rendering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)</span></span>

<span data-ttu-id="58037-186">Ordna om alla visualiseringen på den här sidan som visas i bild.</span><span class="sxs-lookup"><span data-stu-id="58037-186">Rearrange all visualization on this page as shown in figure.</span></span>  
    ![Anslutna bilar - visualiseringar](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

<span data-ttu-id="58037-188">Du har konfigurerat hello ”fordon i åtgärden” realtid rapporten.</span><span class="sxs-lookup"><span data-stu-id="58037-188">You have successfully configured hello “Vehicles in operation” real-time report.</span></span> <span data-ttu-id="58037-189">Du kan fortsätta toocreate hello nästa realtid rapport eller stoppa här och konfigurera hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="58037-189">You can proceed toocreate hello next real-time report or stop here and configure hello dashboard.</span></span> 

### <a name="2-vehicles-requiring-maintenance"></a><span data-ttu-id="58037-190">2. Fordon kräver Underhåll</span><span class="sxs-lookup"><span data-stu-id="58037-190">2. Vehicles Requiring Maintenance</span></span>
<span data-ttu-id="58037-191">Klicka på ![Lägg till](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd en ny rapport, byta namn på den för**”fordon kräver Underhåll”**</span><span class="sxs-lookup"><span data-stu-id="58037-191">Click ![Add](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd a new report, rename it too**“Vehicles Requiring Maintenance”**</span></span>

![Anslutna bilar - fordon kräver Underhåll](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

<span data-ttu-id="58037-193">Välj **vin** fältet och ändra visualiseringen för**kort**.</span><span class="sxs-lookup"><span data-stu-id="58037-193">Select **vin** field and change visualization type too**Card**.</span></span>  
    <span data-ttu-id="58037-194">![Anslutna bilar - Vin kort visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)</span><span class="sxs-lookup"><span data-stu-id="58037-194">![Connected Cars - Vin Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)</span></span>  

<span data-ttu-id="58037-195">Vi har ett fält med namnet ”MaintenanceLabel” i hello dataset.</span><span class="sxs-lookup"><span data-stu-id="58037-195">We have a field named “MaintenanceLabel” In hello dataset.</span></span> <span data-ttu-id="58037-196">Det här fältet kan ha värdet ”0” eller ”1” ”.</span><span class="sxs-lookup"><span data-stu-id="58037-196">This field can have a value of “0” or “1”.”</span></span> <span data-ttu-id="58037-197">Den anges av hello Azure Machine Learning modell etablerats som en del av lösningen och integrerats med hello realtid sökväg.</span><span class="sxs-lookup"><span data-stu-id="58037-197">It is set by hello Azure Machine Learning model provisioned as part of solution and integrated with hello real-time path.</span></span> <span data-ttu-id="58037-198">hello värdet ”1” anger fordon kräver underhåll.</span><span class="sxs-lookup"><span data-stu-id="58037-198">hello value “1” indicates a vehicle requires maintenance.</span></span> 

<span data-ttu-id="58037-199">tooadd en **sidnivå** filter för att visa fordon data, vilket kräver Underhåll:</span><span class="sxs-lookup"><span data-stu-id="58037-199">tooadd a **Page Level** filter for showing vehicles data, which are requiring maintenance:</span></span> 

1. <span data-ttu-id="58037-200">Dra hello **”MaintenanceLabel”** omvandlas **nivå sidfilter**.</span><span class="sxs-lookup"><span data-stu-id="58037-200">Drag hello **“MaintenanceLabel”** field into **Page Level Filters**.</span></span>  
   <span data-ttu-id="58037-201">![Anslutna bilar - sidnivå filter](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)</span><span class="sxs-lookup"><span data-stu-id="58037-201">![Connected Cars - Page Level Filters](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)</span></span>  
2. <span data-ttu-id="58037-202">Klicka på **grundläggande filtrering** menyn som finns längst ned i MaintenanceLabel sidfilter nivå.</span><span class="sxs-lookup"><span data-stu-id="58037-202">Click **Basic Filtering** menu present at bottom of MaintenanceLabel Page Level Filter.</span></span>  
   <span data-ttu-id="58037-203">![Ansluten bilar - grundläggande filtrering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)</span><span class="sxs-lookup"><span data-stu-id="58037-203">![Connected Cars - Basic Filtering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)</span></span>  
3. <span data-ttu-id="58037-204">Ange filtervärdet för**”1”**  </span><span class="sxs-lookup"><span data-stu-id="58037-204">Set its filter value too**“1”**  </span></span>  
   <span data-ttu-id="58037-205">![Anslutna bilar - filtervärdet](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)</span><span class="sxs-lookup"><span data-stu-id="58037-205">![Connected Cars - Filter Value](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)</span></span>  

<span data-ttu-id="58037-206">Klicka på hello tomt område tooadd nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="58037-206">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="58037-207">Välj **grupperat stående stapeldiagram** från visualiseringar</span><span class="sxs-lookup"><span data-stu-id="58037-207">Select **Clustered Column Chart** from visualizations</span></span>  
<span data-ttu-id="58037-208">![Anslutna bilar - Vind kort visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)</span><span class="sxs-lookup"><span data-stu-id="58037-208">![Connected Cars - Vind Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)</span></span>  
<span data-ttu-id="58037-209">![Anslutna bilar - grupperat stående stapeldiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)</span><span class="sxs-lookup"><span data-stu-id="58037-209">![Connected Cars - Clustered Column Chart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)</span></span>

<span data-ttu-id="58037-210">Dra fältet **modellen** till **axel** området **Vin** för**värdet** område.</span><span class="sxs-lookup"><span data-stu-id="58037-210">Drag field **Model** into **Axis** area, **Vin** too**Value** area.</span></span> <span data-ttu-id="58037-211">Sortera visualisering av **antal vin**.</span><span class="sxs-lookup"><span data-stu-id="58037-211">Then sort visualization by **Count of vin**.</span></span>  <span data-ttu-id="58037-212">Ändra diagramtyp **rubrik** för**”fordon kräver underhåll av modell”**</span><span class="sxs-lookup"><span data-stu-id="58037-212">Change chart **Title** too**“Vehicles requiring maintenance by model”**</span></span>  

<span data-ttu-id="58037-213">Dra **vin** fält i **färgmättnad** finns på **fält** ![fält](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) avsnitt i **visualiseringen** fliken</span><span class="sxs-lookup"><span data-stu-id="58037-213">Drag **vin** fields into **Color Saturation** present at **Fields** ![Fields](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) section of **Visualization** tab</span></span>  
<span data-ttu-id="58037-214">![Anslutna bilar - färgmättnad](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)</span><span class="sxs-lookup"><span data-stu-id="58037-214">![Connected Cars - Color Saturation](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)</span></span>  

<span data-ttu-id="58037-215">Ändra **Data färger** i visualiseringar från **Format** avsnitt</span><span class="sxs-lookup"><span data-stu-id="58037-215">Change **Data Colors** in visualizations from **Format** section</span></span>  
<span data-ttu-id="58037-216">Ändra minimifärg till: **F2C812**</span><span class="sxs-lookup"><span data-stu-id="58037-216">Change Minimum color to: **F2C812**</span></span>  
<span data-ttu-id="58037-217">Ändra maxfärg till: **FF6300**</span><span class="sxs-lookup"><span data-stu-id="58037-217">Change Maximum color to: **FF6300**</span></span>  
<span data-ttu-id="58037-218">![Ansluten bilar - ändringar](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)</span><span class="sxs-lookup"><span data-stu-id="58037-218">![Connected Cars - Color Changes](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)</span></span>  
<span data-ttu-id="58037-219">![Ansluten bilar - nya Visualiseringsfärger](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)</span><span class="sxs-lookup"><span data-stu-id="58037-219">![Connected Cars - New Visualization Colors](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)</span></span>  

<span data-ttu-id="58037-220">Klicka på hello tomt område tooadd nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="58037-220">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="58037-221">Välj **klustrade stapeldiagram** dra från visualiseringar, **vin** fältet i **värdet** område, dra **Stad** omvandlas **axel** område.</span><span class="sxs-lookup"><span data-stu-id="58037-221">Select **Clustered column chart** from visualizations, drag **vin** field into **Value** area, drag **City** field into **Axis** area.</span></span> <span data-ttu-id="58037-222">Sortera diagram av **”antal vin”**.</span><span class="sxs-lookup"><span data-stu-id="58037-222">Sort chart by **“Count of vin”**.</span></span> <span data-ttu-id="58037-223">Ändra diagramtyp **rubrik** för**”fordon kräver underhåll av ort”** </span><span class="sxs-lookup"><span data-stu-id="58037-223">Change chart **Title** too**“Vehicles requiring maintenance by city”** </span></span>  
<span data-ttu-id="58037-224">![Anslutna bilar - fordon kräver underhåll av ort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)</span><span class="sxs-lookup"><span data-stu-id="58037-224">![Connected Cars - Vehicles requiring maintenance by city](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)</span></span>  

<span data-ttu-id="58037-225">Klicka på hello tomt område tooadd nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="58037-225">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="58037-226">Välj **flerradiga kort** visualiseringen från visualiseringar, dra **modellen** och **vin** till hello **fält** område.</span><span class="sxs-lookup"><span data-stu-id="58037-226">Select **Multi-Row Card** visualization from visualizations, drag **Model** and **vin** into hello **Fields** area.</span></span>  
<span data-ttu-id="58037-227">![Anslutna bilar - flerradiga-kort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)</span><span class="sxs-lookup"><span data-stu-id="58037-227">![Connected Cars - Multi-Row Card](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)</span></span>    

<span data-ttu-id="58037-228">Flytta alla hello visualiseringen hello slutgiltiga rapporten ser ut som följer:</span><span class="sxs-lookup"><span data-stu-id="58037-228">Rearranging all of hello visualization, hello final report looks as follows:</span></span>  
![Anslutna bilar - flerradiga-kort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

<span data-ttu-id="58037-230">Du har konfigurerat hello ”fordon kräver Underhåll” realtid rapporten.</span><span class="sxs-lookup"><span data-stu-id="58037-230">You have successfully configured hello “Vehicles Requiring Maintenance” real-time report.</span></span> <span data-ttu-id="58037-231">Du kan fortsätta toocreate hello nästa realtid rapport eller stoppa här och konfigurera hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="58037-231">You can proceed toocreate hello next real-time report or stop here and configure hello dashboard.</span></span> 

### <a name="3-vehicles-health-statistics"></a><span data-ttu-id="58037-232">3. Fordon hälsostatistik</span><span class="sxs-lookup"><span data-stu-id="58037-232">3. Vehicles Health Statistics</span></span>
<span data-ttu-id="58037-233">Klicka på ![Lägg till](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd ny rapport, byta namn på den för**”fordon hälsostatistik”**</span><span class="sxs-lookup"><span data-stu-id="58037-233">Click ![Add](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd new report, rename it too**“Vehicles Health Statistics”**</span></span>  

<span data-ttu-id="58037-234">Välj **mätaren** visualiseringen från visualiseringar, dra hello **hastighet** omvandlas **värde, minimalt värde maxvärdet** områden.</span><span class="sxs-lookup"><span data-stu-id="58037-234">Select **Gauge** visualization from visualizations, then drag hello **Speed** field into **Value, Minimum Value, Maximum Value** areas.</span></span>  
<span data-ttu-id="58037-235">![Anslutna bilar - flerradiga-kort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)</span><span class="sxs-lookup"><span data-stu-id="58037-235">![Connected Cars - Multi-Row Card](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)</span></span>  

<span data-ttu-id="58037-236">Ändra hello standard aggregering av **hastighet** i **värdet området** för**Genomsnittlig**</span><span class="sxs-lookup"><span data-stu-id="58037-236">Change hello default aggregation of **speed** in **Value area** too**Average**</span></span> 

<span data-ttu-id="58037-237">Ändra hello standard aggregering av **hastighet** i **minsta område** för**minsta**</span><span class="sxs-lookup"><span data-stu-id="58037-237">Change hello default aggregation of **speed** in **Minimum area** too**Minimum**</span></span>

<span data-ttu-id="58037-238">Ändra hello standard aggregering av **hastighet** i **maximalt området** för**maximala**</span><span class="sxs-lookup"><span data-stu-id="58037-238">Change hello default aggregation of **speed** in **Maximum area** too**Maximum**</span></span>

![Anslutna bilar - flerradiga-kort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

<span data-ttu-id="58037-240">Byt namn på hello **mätaren rubrik** för**”genomsnittlig hastighet”**</span><span class="sxs-lookup"><span data-stu-id="58037-240">Rename hello **Gauge Title** too**“Average speed”**</span></span> 

![Anslutna bilar - mätare](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

<span data-ttu-id="58037-242">Klicka på hello tomt område tooadd nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="58037-242">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="58037-243">Lägga till en **mätaren** för **genomsnittlig motorolja**, **genomsnittlig bränsle**, och **genomsnittlig motorn tempererade**.</span><span class="sxs-lookup"><span data-stu-id="58037-243">Similarly add a **Gauge** for **average engine oil**, **average fuel**, and **average engine temperate**.</span></span>  

<span data-ttu-id="58037-244">Ändra hello standard aggregering av fälten i varje mätaren enligt ovan stegen i **”genomsnittlig hastighet”** mätare.</span><span class="sxs-lookup"><span data-stu-id="58037-244">Change hello default aggregation of fields in each gauge as per above steps in **“Average speed”** gauge.</span></span>

![Anslutna bilar - mätare](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

<span data-ttu-id="58037-246">Klicka på hello tomt område tooadd nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="58037-246">Click hello blank area tooadd new visualization.</span></span>

<span data-ttu-id="58037-247">Välj **linjediagram och grupperat stående stapeldiagram** från visualiseringar, dra **Stad** omvandlas **delade axel**, dra **hastighet**, **fälten tirepressure och engineoil** till **kolumnvärdena** område, ändra deras aggregeringstypen för**genomsnittlig**.</span><span class="sxs-lookup"><span data-stu-id="58037-247">Select **Line and Clustered Column Chart** from visualizations, then drag **City** field into **Shared Axis**, drag **speed**, **tirepressure and engineoil fields** into **Column Values** area, change their aggregation type too**Average**.</span></span> 

<span data-ttu-id="58037-248">Dra hello **engineTemperature** omvandlas **radvärden** område, ändra hello aggregeringstypen för**genomsnittlig**.</span><span class="sxs-lookup"><span data-stu-id="58037-248">Drag hello **engineTemperature** field into **Line Values** area, change hello  aggregation type too**Average**.</span></span> 

![Anslutna bilar - visualiseringar fält](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

<span data-ttu-id="58037-250">Ändra hello diagram **rubrik** för**”genomsnittlig hastighet, däck tryck, motorolja och motorn temperatur”**.</span><span class="sxs-lookup"><span data-stu-id="58037-250">Change hello chart **Title** too**“Average speed, tire pressure, engine oil and engine temperature”**.</span></span>  

![Anslutna bilar - visualiseringar fält](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

<span data-ttu-id="58037-252">Klicka på hello tomt område tooadd nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="58037-252">Click hello blank area tooadd new visualization.</span></span>

<span data-ttu-id="58037-253">Välj **Treemap** visualiseringen från visualiseringar, dra hello **modellen** omvandlas hello **grupp** området och dra hello fältet  **MaintenanceProbability** till hello **värden** område.</span><span class="sxs-lookup"><span data-stu-id="58037-253">Select **Treemap** visualization from visualizations, drag hello **Model** field into hello **Group** area, and drag hello field **MaintenanceProbability** into hello **Values** area.</span></span>

<span data-ttu-id="58037-254">Ändra hello diagram **rubrik** för**”Vehicle modeller som kräver Underhåll”**.</span><span class="sxs-lookup"><span data-stu-id="58037-254">Change hello chart **Title** too**“Vehicle models requiring maintenance”**.</span></span>

![Ansluten bilar - ändra diagrammets rubrik](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

<span data-ttu-id="58037-256">Klicka på hello tomt område tooadd nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="58037-256">Click hello blank area tooadd new visualization.</span></span>

<span data-ttu-id="58037-257">Välj **100% staplad liggande diagram** dra hello från visualisering, **Stad** omvandlas hello **axel** området och dra hello **MaintenanceProbability**, **RecallProbability** fält i hello **värdet** område.</span><span class="sxs-lookup"><span data-stu-id="58037-257">Select **100% Stacked Bar Chart** from visualization, drag hello **city** field into hello **Axis** area, and drag hello **MaintenanceProbability**, **RecallProbability** fields into hello **Value** area.</span></span>

![Anslutna bilar - Lägg till nya visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

<span data-ttu-id="58037-259">Klicka på **Format**väljer **Data färger**, och ange hello **MaintenanceProbability** färgvärden toohello **”F2C80F”**.</span><span class="sxs-lookup"><span data-stu-id="58037-259">Click **Format**, select **Data Colors**, and set hello **MaintenanceProbability** color toohello value **“F2C80F”**.</span></span>

<span data-ttu-id="58037-260">Ändra hello **rubrik** av hello diagram för**”sannolikheten för Vehicle underhåll och återkalla av ort”**.</span><span class="sxs-lookup"><span data-stu-id="58037-260">Change hello **Title** of hello chart too**“Probability of Vehicle Maintenance & Recall by City”**.</span></span>

![Anslutna bilar - Lägg till nya visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

<span data-ttu-id="58037-262">Klicka på hello tomt område tooadd nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="58037-262">Click hello blank area tooadd new visualization.</span></span>

<span data-ttu-id="58037-263">Välj **ytdiagram** dra hello från visualiseringen från visualiseringar **modellen** omvandlas hello **axel** området och dra hello **engineOil, tirepressure, hastighet och MaintenanceProbability** fält i hello **värden** område.</span><span class="sxs-lookup"><span data-stu-id="58037-263">Select **Area Chart** from visualization from visualizations, drag hello **Model** field into hello **Axis** area, and drag hello **engineOil, tirepressure, speed and MaintenanceProbability** fields into hello **Values** area.</span></span> <span data-ttu-id="58037-264">Ändra sina sammansättningstyp för**”genomsnittliga”**.</span><span class="sxs-lookup"><span data-stu-id="58037-264">Change their aggregation type too**“Average”**.</span></span> 

![Ansluten bilar - ändra sammansättningstyp](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

<span data-ttu-id="58037-266">Ändra hello rubrik hello diagram för**”genomsnittlig motorolja, tröttnar sannolikheten för hög belastning, hastighet och underhåll av modell”**.</span><span class="sxs-lookup"><span data-stu-id="58037-266">Change hello title of hello chart too**“Average engine oil, tire pressure, speed and maintenance probability by model”**.</span></span>

![Ansluten bilar - ändra diagrammets rubrik](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

<span data-ttu-id="58037-268">Klicka på hello tomt område tooadd nya visualiseringen:</span><span class="sxs-lookup"><span data-stu-id="58037-268">Click hello blank area tooadd new visualization:</span></span>

1. <span data-ttu-id="58037-269">Välj **punktdiagram** visualiseringen från visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="58037-269">Select **Scatter Chart** visualization from visualizations.</span></span>
2. <span data-ttu-id="58037-270">Dra hello **modellen** omvandlas hello **information** och **förklaring** område.</span><span class="sxs-lookup"><span data-stu-id="58037-270">Drag hello **Model** field into hello **Details** and **Legend** area.</span></span>
3. <span data-ttu-id="58037-271">Dra hello **bränsle** omvandlas hello **x-axeln** område, ändra hello aggregering för**genomsnittlig**.</span><span class="sxs-lookup"><span data-stu-id="58037-271">Drag hello **fuel** field into hello **X-Axis** area, change hello aggregation too**Average**.</span></span>
4. <span data-ttu-id="58037-272">Dra **engineTemparature** till **y-axeln området**, ändra hello aggregering för**Genomsnittlig**</span><span class="sxs-lookup"><span data-stu-id="58037-272">Drag **engineTemparature** into **Y-Axis area**, change hello aggregation too**Average**</span></span>
5. <span data-ttu-id="58037-273">Dra hello **vin** omvandlas hello **storlek** område.</span><span class="sxs-lookup"><span data-stu-id="58037-273">Drag hello **vin** field into hello **Size** area.</span></span>

![Anslutna bilar - Lägg till nya visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

<span data-ttu-id="58037-275">Ändra hello diagram **rubrik** för**”medelvärden bränsle, motorn temperatur av modell”**.</span><span class="sxs-lookup"><span data-stu-id="58037-275">Change hello chart **Title** too**“Averages of Fuel, Engine Temperature by Model”**.</span></span>

![Ansluten bilar - ändra diagrammets rubrik](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

<span data-ttu-id="58037-277">hello slutgiltiga rapporten ser ut som nedan.</span><span class="sxs-lookup"><span data-stu-id="58037-277">hello final report will look like as shown below.</span></span>

![Ansluten bilar-slutgiltig rapport](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-hello-reports-toohello-real-time-dashboard"></a><span data-ttu-id="58037-279">PIN-kod visualiseringar hello rapporter toohello realtid instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="58037-279">Pin visualizations from hello reports toohello real-time dashboard</span></span>
<span data-ttu-id="58037-280">Skapa en tom instrumentpanel genom att klicka på nästa tooDashboards för hello plus ikon.</span><span class="sxs-lookup"><span data-stu-id="58037-280">Create a blank dashboard by clicking on hello plus icon next tooDashboards.</span></span> <span data-ttu-id="58037-281">Du kan kalla den ”Vehicle telemetri instrumentpanelen”</span><span class="sxs-lookup"><span data-stu-id="58037-281">You can name it “Vehicle Telemetry Analytics Dashboard”</span></span>

![Anslutna bilar-instrumentpanelen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

<span data-ttu-id="58037-283">Fäst hello visualiseringen från hello ovan rapporter toohello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="58037-283">Pin hello visualization from hello above reports toohello dashboard.</span></span> 

![Anslutna bilar-instrumentpanelen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

<span data-ttu-id="58037-285">hello instrumentpanelen ska se ut så här när alla hello tre rapporter skapas och hello motsvarande visualiseringar är fäst toohello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="58037-285">hello dashboard should look as follows when all hello three reports are created and hello corresponding visualizations are pinned toohello dashboard.</span></span> <span data-ttu-id="58037-286">Om du inte har skapat alla hello rapporter instrumentpanelen kan se annorlunda ut.</span><span class="sxs-lookup"><span data-stu-id="58037-286">If you have not created all hello reports, your dashboard could look different.</span></span> 

![Anslutna bilar-instrumentpanelen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

<span data-ttu-id="58037-288">Grattis!</span><span class="sxs-lookup"><span data-stu-id="58037-288">Congratulations!</span></span> <span data-ttu-id="58037-289">Du har skapat hello realtid instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="58037-289">You have successfully created hello real-time dashboard.</span></span> <span data-ttu-id="58037-290">När du fortsätter tooexecute CarEventGenerator.exe och RealtimeDashboardApp.exe, bör du se live uppdateringar på hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="58037-290">As you continue tooexecute CarEventGenerator.exe and RealtimeDashboardApp.exe, you should see live updates on hello dashboard.</span></span> <span data-ttu-id="58037-291">Det bör ta cirka 10 too15 minuter toocomplete hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="58037-291">It should take about 10 too15 minutes toocomplete hello following steps.</span></span>

## <a name="setup-power-bi-batch-processing-dashboard"></a><span data-ttu-id="58037-292">Installera Power BI batch-bearbetning instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="58037-292">Setup Power BI batch processing dashboard</span></span>
> [!NOTE]
> <span data-ttu-id="58037-293">Det tar ca två timmar (från hello slutförande av hello distribution) för hello slutet tooend batchbearbetning toofinish pipelinekörningen och bearbeta ett år som skapas.</span><span class="sxs-lookup"><span data-stu-id="58037-293">It takes about two hours (from hello successful completion of hello deployment) for hello end tooend batch processing pipeline toofinish execution and process a year worth of generated data.</span></span> <span data-ttu-id="58037-294">Vänta så hello bearbetning toofinish innan du fortsätter med hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="58037-294">So wait for hello processing toofinish before proceeding with hello next steps.</span></span> 
> 
> 

<span data-ttu-id="58037-295">**Hämta hello Power BI designer-fil**</span><span class="sxs-lookup"><span data-stu-id="58037-295">**Download hello Power BI designer file**</span></span>

* <span data-ttu-id="58037-296">En förkonfigurerad Power BI designer fil ingår som en del av hello distribution manuell åtgärd instruktioner</span><span class="sxs-lookup"><span data-stu-id="58037-296">A pre-configured Power BI designer file is included as part of hello deployment Manual Operation Instructions</span></span>
* <span data-ttu-id="58037-297">Leta efter 2.</span><span class="sxs-lookup"><span data-stu-id="58037-297">Look for 2.</span></span> <span data-ttu-id="58037-298">Installationsprogrammet PowerBI batch bearbetning instrumentpanelen kan du hämta hello PowerBI-mall för batchbearbetning instrumentpanelen namnet **ConnectedCarsPbiReport.pbix**.</span><span class="sxs-lookup"><span data-stu-id="58037-298">Setup PowerBI batch processing dashboard You can download hello PowerBI template for batch processing dashboard here called **ConnectedCarsPbiReport.pbix**.</span></span>
* <span data-ttu-id="58037-299">Spara lokalt</span><span class="sxs-lookup"><span data-stu-id="58037-299">Save locally</span></span>

<span data-ttu-id="58037-300">**Konfigurera Power BI-rapporter**</span><span class="sxs-lookup"><span data-stu-id="58037-300">**Configure Power BI reports**</span></span>

* <span data-ttu-id="58037-301">Öppna hello designer filen '**ConnectedCarsPbiReport.pbix**' med hjälp av Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="58037-301">Open hello designer file ‘**ConnectedCarsPbiReport.pbix**’ using Power BI Desktop.</span></span> <span data-ttu-id="58037-302">Om du inte redan har, installerar hello Power BI Desktop från [Power BI Desktop installera](http://www.microsoft.com/download/details.aspx?id=45331).</span><span class="sxs-lookup"><span data-stu-id="58037-302">If you do not already have, install hello Power BI Desktop from [Power BI Desktop install](http://www.microsoft.com/download/details.aspx?id=45331).</span></span> 
* <span data-ttu-id="58037-303">Klicka på hello **redigera frågor**.</span><span class="sxs-lookup"><span data-stu-id="58037-303">Click hello **Edit Queries**.</span></span>

![Redigera Power BI-fråga](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

* <span data-ttu-id="58037-305">Dubbelklicka på hello **källa**.</span><span class="sxs-lookup"><span data-stu-id="58037-305">Double-click hello **Source**.</span></span>

![Ange Power BI-källa](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

* <span data-ttu-id="58037-307">Uppdatera Server-anslutningssträngen med hello Azure SQL-server som har etablerats som en del av hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="58037-307">Update Server connection string with hello Azure SQL server that got provisioned as part of hello deployment.</span></span>  <span data-ttu-id="58037-308">Leta i hello manuell åtgärd anvisningarna under</span><span class="sxs-lookup"><span data-stu-id="58037-308">Look in hello Manual Operation Instructions under</span></span> 

    4. <span data-ttu-id="58037-309">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="58037-309">Azure SQL Database</span></span>
    
    * <span data-ttu-id="58037-310">Server: somethingsrv.database.windows.net</span><span class="sxs-lookup"><span data-stu-id="58037-310">Server: somethingsrv.database.windows.net</span></span>
    * <span data-ttu-id="58037-311">Databas: connectedcar</span><span class="sxs-lookup"><span data-stu-id="58037-311">Database: connectedcar</span></span>
    * <span data-ttu-id="58037-312">Användarnamn: användarnamn</span><span class="sxs-lookup"><span data-stu-id="58037-312">Username: username</span></span>
    * <span data-ttu-id="58037-313">Lösenord: Du kan hantera ditt lösenord för SQL server från Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="58037-313">Password: You can manage your SQL server password from Azure portal</span></span>

* <span data-ttu-id="58037-314">Lämna **databasen** som *connectedcar*.</span><span class="sxs-lookup"><span data-stu-id="58037-314">Leave **Database** as *connectedcar*.</span></span>

![Ange Power BI-databasen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

* <span data-ttu-id="58037-316">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="58037-316">Click **OK**.</span></span>
* <span data-ttu-id="58037-317">Du ser **Windows-autentiseringsuppgifter** fliken markerad som standard kan ändra det för**databasen autentiseringsuppgifter** genom att klicka på **databasen** fliken längst till höger.</span><span class="sxs-lookup"><span data-stu-id="58037-317">You will see **Windows credential** tab selected by default, change it too**Database credentials** by clicking on **Database** tab at right.</span></span>
* <span data-ttu-id="58037-318">Ange hello **användarnamn** och **lösenord** i din Azure SQL Database som angavs under installationen för distribution.</span><span class="sxs-lookup"><span data-stu-id="58037-318">Provide hello **Username** and **Password** of your Azure SQL Database that was specified during its deployment setup.</span></span>

![Ange Databasautentiseringsuppgifter för](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

* <span data-ttu-id="58037-320">Klicka på **ansluta**</span><span class="sxs-lookup"><span data-stu-id="58037-320">Click **Connect**</span></span>
* <span data-ttu-id="58037-321">Upprepa hello ovanstående steg för varje hello tre återstående frågor finns i högra fönstret och uppdatera sedan hello anslutningsinformation om datakälla.</span><span class="sxs-lookup"><span data-stu-id="58037-321">Repeat hello above steps for each of hello three remaining queries present at right pane, and then update hello data source connection details.</span></span>
* <span data-ttu-id="58037-322">Klicka på **Stäng och läsa in**.</span><span class="sxs-lookup"><span data-stu-id="58037-322">Click **Close and Load**.</span></span> <span data-ttu-id="58037-323">Power BI Desktop filen datauppsättningar är anslutna tooSQL Azure databastabeller.</span><span class="sxs-lookup"><span data-stu-id="58037-323">Power BI Desktop file datasets are connected tooSQL Azure Database tables.</span></span>
* <span data-ttu-id="58037-324">**Stäng** Power BI Desktop-fil.</span><span class="sxs-lookup"><span data-stu-id="58037-324">**Close** Power BI Desktop file.</span></span>

![Stäng Power BI desktop](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

* <span data-ttu-id="58037-326">Klicka på **spara** knappen toosave hello ändras.</span><span class="sxs-lookup"><span data-stu-id="58037-326">Click **Save** button toosave hello changes.</span></span> 

<span data-ttu-id="58037-327">Du har nu konfigurerat alla hello rapporter motsvarande toohello batch bearbetning sökväg i hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="58037-327">You have now configured all hello reports corresponding toohello batch processing path in hello solution.</span></span> 

## <a name="upload-toopowerbicom"></a><span data-ttu-id="58037-328">Överför för*powerbi.com*</span><span class="sxs-lookup"><span data-stu-id="58037-328">Upload too*powerbi.com*</span></span>
1. <span data-ttu-id="58037-329">Navigera toohello Power BI-webbportalen på http://powerbi.com och logga in.</span><span class="sxs-lookup"><span data-stu-id="58037-329">Navigate toohello Power BI web portal at http://powerbi.com and login.</span></span>
2. <span data-ttu-id="58037-330">Klicka på **hämta Data**</span><span class="sxs-lookup"><span data-stu-id="58037-330">Click **Get Data**</span></span>  
3. <span data-ttu-id="58037-331">Överför hello Power BI Desktop-fil.</span><span class="sxs-lookup"><span data-stu-id="58037-331">Upload hello Power BI Desktop File.</span></span>  
4. <span data-ttu-id="58037-332">tooupload, klickar du på **hämta Data -> hämta filer -> lokal fil**</span><span class="sxs-lookup"><span data-stu-id="58037-332">tooupload, click **Get Data -> Files Get -> Local file**</span></span>  
5. <span data-ttu-id="58037-333">Navigera toohello **”**ConnectedCarsPbiReport.pbix**”**</span><span class="sxs-lookup"><span data-stu-id="58037-333">Navigate toohello **“**ConnectedCarsPbiReport.pbix**”**</span></span>  
6. <span data-ttu-id="58037-334">När hello-filen har överförts, kommer du att navigera tillbaka tooyour Power BI-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="58037-334">Once hello file is uploaded, you will be navigated back tooyour Power BI work space.</span></span>  

<span data-ttu-id="58037-335">En datamängd, rapporten och en tom instrumentpanel skapas för dig.</span><span class="sxs-lookup"><span data-stu-id="58037-335">A dataset, report and a blank dashboard will be created for you.</span></span>  

<span data-ttu-id="58037-336">PIN-kod diagram tooa ny instrumentpanel som kallas **Vehicle telemetri instrumentpanelen** i **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="58037-336">Pin charts tooa new dashboard called **Vehicle Telemetry Analytics Dashboard** in **Power BI**.</span></span> <span data-ttu-id="58037-337">Klicka på hello tomma instrumentpanelen skapade ovan och gå sedan toohello **rapporter** avsnittet klickar du på hello nyligen har överförts till rapporten.</span><span class="sxs-lookup"><span data-stu-id="58037-337">Click hello blank dashboard created above and then navigate toohello **Reports** section click hello newly uploaded report.</span></span>  

![Vehicle telemetri Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

<span data-ttu-id="58037-339">**Hello rapport har sex sidor:**</span><span class="sxs-lookup"><span data-stu-id="58037-339">**Note hello report has six pages:**</span></span>  
<span data-ttu-id="58037-340">Sidan 1: Vehicle densitet</span><span class="sxs-lookup"><span data-stu-id="58037-340">Page 1: Vehicle density</span></span>  
<span data-ttu-id="58037-341">Sidan 2: Realtid vehicle hälsa</span><span class="sxs-lookup"><span data-stu-id="58037-341">Page 2: Real-time vehicle health</span></span>  
<span data-ttu-id="58037-342">Sidan 3: Aggressivt drivs fordon</span><span class="sxs-lookup"><span data-stu-id="58037-342">Page 3: Aggressively Driven Vehicles</span></span>   
<span data-ttu-id="58037-343">Sidan 4: Återkallas fordon</span><span class="sxs-lookup"><span data-stu-id="58037-343">Page 4: Recalled vehicles</span></span>  
<span data-ttu-id="58037-344">Sidan 5: Bränsle effektivt drivs fordon</span><span class="sxs-lookup"><span data-stu-id="58037-344">Page 5: Fuel Efficiently Driven Vehicles</span></span>  
<span data-ttu-id="58037-345">Sidan 6: Contoso-logotyp</span><span class="sxs-lookup"><span data-stu-id="58037-345">Page 6: Contoso Logo</span></span>  

![Anslutna bilar Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

<span data-ttu-id="58037-347">**Från sidan 3**, fästa hello följande:</span><span class="sxs-lookup"><span data-stu-id="58037-347">**From Page 3**, pin hello following:</span></span>  

1. <span data-ttu-id="58037-348">Antal VIN</span><span class="sxs-lookup"><span data-stu-id="58037-348">Count of VIN</span></span>  
   ![Anslutna bilar Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 
2. <span data-ttu-id="58037-350">Aggressivt drivs fordon av modellen – vattenfallet diagram</span><span class="sxs-lookup"><span data-stu-id="58037-350">Aggressively driven vehicles by model – Waterfall chart</span></span>  
   ![Vehicle telemetri - PIN-kod diagram 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

<span data-ttu-id="58037-352">**Från sidan 5**, fästa hello följande:</span><span class="sxs-lookup"><span data-stu-id="58037-352">**From Page 5**, pin hello following:</span></span> 

1. <span data-ttu-id="58037-353">Antal vin</span><span class="sxs-lookup"><span data-stu-id="58037-353">Count of vin</span></span>    
   ![Vehicle telemetri - PIN-kod diagram 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2. <span data-ttu-id="58037-355">Bränsle effektivt fordon av modell: grupperat stående stapeldiagram</span><span class="sxs-lookup"><span data-stu-id="58037-355">Fuel efficient vehicles by model: Clustered column chart</span></span>  
   ![Vehicle telemetri - PIN-kod diagram 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

<span data-ttu-id="58037-357">**Från sidan 4**, fästa hello följande:</span><span class="sxs-lookup"><span data-stu-id="58037-357">**From Page 4**, pin hello following:</span></span>  

1. <span data-ttu-id="58037-358">Antal vin</span><span class="sxs-lookup"><span data-stu-id="58037-358">Count of vin</span></span>  
   ![Vehicle telemetri - PIN-kod diagram 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 
2. <span data-ttu-id="58037-360">Återkallade fordon efter ort, modell: Treemap</span><span class="sxs-lookup"><span data-stu-id="58037-360">Recalled vehicles by city, model: Treemap</span></span>  
   ![Vehicle telemetri - PIN-kod diagram 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

<span data-ttu-id="58037-362">**Från sidan 6**, fästa hello följande:</span><span class="sxs-lookup"><span data-stu-id="58037-362">**From Page 6**, pin hello following:</span></span>  

1. <span data-ttu-id="58037-363">Contoso motorer-logotyp</span><span class="sxs-lookup"><span data-stu-id="58037-363">Contoso Motors logo</span></span>  
   ![Vehicle telemetri - PIN-kod diagram 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

<span data-ttu-id="58037-365">**Ordna hello instrumentpanelen**</span><span class="sxs-lookup"><span data-stu-id="58037-365">**Organize hello dashboard**</span></span>  

1. <span data-ttu-id="58037-366">Navigera toohello instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="58037-366">Navigate toohello dashboard</span></span>
2. <span data-ttu-id="58037-367">Hovra över varje diagram och Byt baserat på hello naming i hello komplett instrumentpanel bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="58037-367">Hover over each chart and rename it based on hello naming provided in hello complete dashboard image below.</span></span> <span data-ttu-id="58037-368">Även flytta hello diagram runt toolook som hello instrumentpanelen nedan.</span><span class="sxs-lookup"><span data-stu-id="58037-368">Also move hello charts around toolook like hello dashboard below.</span></span>  
   ![Vehicle telemetri - ordna instrumentpanelen 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
   ![Vehicle telemetri Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3. <span data-ttu-id="58037-371">Om du har skapat alla hello rapporter som anges i det här dokumentet hello sista slutförda instrumentpanelen bör se ut som hello följande bild.</span><span class="sxs-lookup"><span data-stu-id="58037-371">If you have created all hello reports as mentioned in this document, hello final completed dashboard should look like hello following figure.</span></span> 

![Vehicle telemetri - ordna instrumentpanelen 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

<span data-ttu-id="58037-373">Grattis!</span><span class="sxs-lookup"><span data-stu-id="58037-373">Congratulations!</span></span> <span data-ttu-id="58037-374">Du har skapat hello rapporter och hello instrumentpanelen toogain realtid, förutsägbara och batch insikter om vehicle hälsa och köra vanor.</span><span class="sxs-lookup"><span data-stu-id="58037-374">You have successfully created hello reports and hello dashboard toogain real-time, predictive and batch insights on vehicle health and driving habits.</span></span>  
