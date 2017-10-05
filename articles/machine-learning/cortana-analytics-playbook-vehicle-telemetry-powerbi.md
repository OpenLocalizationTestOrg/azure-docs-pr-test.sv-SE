---
title: "Power BI-instrumentpanelen för hälsotillstånd vehicle och andra vanor - Azure | Microsoft Docs"
description: "Använda funktionerna i Cortana Intelligence och få insikter om i realtid och förutsägbara på vehicle hälsa och köra vanor."
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
ms.openlocfilehash: f880aceb1657ffdfe909b73f175b9673d9ab02cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a><span data-ttu-id="e9916-103">Vehicle telemetri analytics lösning mallen Power BI-instrumentpanel instruktioner</span><span class="sxs-lookup"><span data-stu-id="e9916-103">Vehicle telemetry analytics solution template Power BI Dashboard setup instructions</span></span>
<span data-ttu-id="e9916-104">Detta **menyn** länkar till kapitlen i den här playbook.</span><span class="sxs-lookup"><span data-stu-id="e9916-104">This **menu** links to the chapters in this playbook.</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

<span data-ttu-id="e9916-105">Vehicle telemetri Analytics lösningen visar hur bil hos återförsäljarna, bil tillverkare och försäkringsbolag kan utnyttja funktionerna i Cortana Intelligence och insyn i realtid och förutsägbara på vehicle hälsa och intresseväckande vanor enhet förbättringar i området för kunden får, R & D och marknadsföringskampanjer.</span><span class="sxs-lookup"><span data-stu-id="e9916-105">The Vehicle Telemetry Analytics solution showcases how car dealerships, automobile manufacturers and insurance companies can leverage the capabilities of Cortana Intelligence to gain real-time and predictive insights on vehicle health and driving habits to drive improvements in the area of customer experience, R&D and marketing campaigns.</span></span> <span data-ttu-id="e9916-106">Det här dokumentet innehåller steg-för-steg-anvisningar om hur du kan konfigurera en Power BI-rapporter och instrumentpanelen när lösningen har distribuerats i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e9916-106">This document contains step by step instructions on how you can configure the Power BI reports and dashboard once the solution is deployed in your subscription.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e9916-107">Krav</span><span class="sxs-lookup"><span data-stu-id="e9916-107">Prerequisites</span></span>
1. <span data-ttu-id="e9916-108">Distribuera den [telemetri Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) lösning</span><span class="sxs-lookup"><span data-stu-id="e9916-108">Deploy the [Telemetry Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) solution</span></span>  
2. [<span data-ttu-id="e9916-109">Installera Microsoft Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="e9916-109">Install Microsoft Power BI Desktop</span></span>](http://www.microsoft.com/download/details.aspx?id=45331)
3. <span data-ttu-id="e9916-110">En [Azure-prenumeration](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e9916-110">An [Azure subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="e9916-111">Om du inte har en Azure-prenumeration, komma igång med Azure kostnadsfri prenumeration</span><span class="sxs-lookup"><span data-stu-id="e9916-111">If you don't have an Azure subscription, get started with Azure free subscription</span></span>
4. <span data-ttu-id="e9916-112">Microsoft Power BI-konto</span><span class="sxs-lookup"><span data-stu-id="e9916-112">Microsoft Power BI account</span></span>

## <a name="cortana-intelligence-suite-components"></a><span data-ttu-id="e9916-113">Cortana Intelligence Suite-komponenter</span><span class="sxs-lookup"><span data-stu-id="e9916-113">Cortana Intelligence Suite Components</span></span>
<span data-ttu-id="e9916-114">Som en del av lösningsmall Vehicle telemetri Analytics distribueras följande Cortana Intelligence-tjänster i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e9916-114">As part of the Vehicle Telemetry Analytics solution template, the following Cortana Intelligence services are deployed in your subscription.</span></span>

* <span data-ttu-id="e9916-115">**Event Hub** för att föra in miljontals vehicle telemetriska händelser i Azure.</span><span class="sxs-lookup"><span data-stu-id="e9916-115">**Event Hub** for ingesting millions of vehicle telemetry events into Azure.</span></span>
* <span data-ttu-id="e9916-116">**Strömma Analytics** för att få realtidsinsikter på vehicle hälsa och kvarstår dessa data till långsiktig lagring för bättre batch analytics.</span><span class="sxs-lookup"><span data-stu-id="e9916-116">**Stream Analytics** for gaining real-time insights on vehicle health and persists that data into long-term storage for richer batch analytics.</span></span>
* <span data-ttu-id="e9916-117">**Machine Learning** för identifiering av avvikelse i realtid och batchbearbetning och få förutsägande insikter.</span><span class="sxs-lookup"><span data-stu-id="e9916-117">**Machine Learning** for anomaly detection in real-time and batch processing to gain predictive insights.</span></span>
* <span data-ttu-id="e9916-118">**HDInsight** utnyttjas för att omvandla data i skala</span><span class="sxs-lookup"><span data-stu-id="e9916-118">**HDInsight** is leveraged to transform data at scale</span></span>
* <span data-ttu-id="e9916-119">**Data Factory** hanterar orchestration, schemaläggning, resurshantering och övervakning av batch-bearbetning-pipeline.</span><span class="sxs-lookup"><span data-stu-id="e9916-119">**Data Factory** handles orchestration, scheduling, resource management and monitoring of the batch processing pipeline.</span></span>

<span data-ttu-id="e9916-120">**Power BI** ger den här lösningen en omfattande instrumentpanel för data i realtid och förutsägelseanalys visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="e9916-120">**Power BI** gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span> 

<span data-ttu-id="e9916-121">Lösningen använder två olika datakällor: **simulerade vehicle signaler och diagnostik dataset** och **vehicle katalogen**.</span><span class="sxs-lookup"><span data-stu-id="e9916-121">The solution uses two different data sources: **Simulated vehicle signals and diagnostic dataset** and **vehicle catalog**.</span></span>

<span data-ttu-id="e9916-122">Vehicle telematik simulator ingår som en del av den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="e9916-122">A vehicle telematics simulator is included as part of this solution.</span></span> <span data-ttu-id="e9916-123">Den genererar diagnostisk information och signalerar till motsvarande tillstånd för programuppdatering och andra mönster vid en viss tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="e9916-123">It emits diagnostic information and signals corresponding to the state of the vehicle and driving pattern at a given point in time.</span></span> 

<span data-ttu-id="e9916-124">Vehicle katalogen är referens dataset som innehåller Registreringsnumret för Modellmappning</span><span class="sxs-lookup"><span data-stu-id="e9916-124">The Vehicle Catalog is a reference dataset containing VIN to model mapping</span></span>

## <a name="power-bi-dashboard-preparation"></a><span data-ttu-id="e9916-125">Förberedelse av Power BI-instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="e9916-125">Power BI Dashboard Preparation</span></span>
### <a name="setup-power-bi-real-time-dashboard"></a><span data-ttu-id="e9916-126">Konfigurera realtid Power BI-instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="e9916-126">Setup Power BI Real-Time Dashboard</span></span>

<span data-ttu-id="e9916-127">**Starta programmet realtid instrumentpanelen** när distributionen är klar bör du följa anvisningarna för manuell åtgärd</span><span class="sxs-lookup"><span data-stu-id="e9916-127">**Start the real-time dashboard application** Once the deployment is completed, you should follow the Manual Operation Instructions</span></span>

* <span data-ttu-id="e9916-128">Hämta realtid instrumentpanelen programmet RealtimeDashboardApp.zip och packa upp den.</span><span class="sxs-lookup"><span data-stu-id="e9916-128">Download real-time dashboard application RealtimeDashboardApp.zip, and unzip it.</span></span>
*  <span data-ttu-id="e9916-129">Öppna appen konfigurationsfilen RealtimeDashboardApp.exe.config, Ersätt appSettings för Eventhub, Blob Storage och ML service-anslutningar med värden i manuell åtgärd instruktioner och spara ändringarna i den uppackade mappen.</span><span class="sxs-lookup"><span data-stu-id="e9916-129">In the unzipped folder, open app config file 'RealtimeDashboardApp.exe.config', replace appSettings for Eventhub, Blob Storage, and ML service connections with the values in the Manual Operation Instructions, and save your changes.</span></span>
* <span data-ttu-id="e9916-130">Kör program RealtimeDashboardApp.exe.</span><span class="sxs-lookup"><span data-stu-id="e9916-130">Run application RealtimeDashboardApp.exe.</span></span> <span data-ttu-id="e9916-131">Ett inloggningsfönster kommer popup-, ange din giltiga PowerBI-autentiseringsuppgifter och klicka på den **acceptera** knappen.</span><span class="sxs-lookup"><span data-stu-id="e9916-131">A login window will pop up, provide your valid PowerBI credentials and click the **Accept** button.</span></span> <span data-ttu-id="e9916-132">Sedan startar appen.</span><span class="sxs-lookup"><span data-stu-id="e9916-132">Then the app will start to run.</span></span>

   ![Logga in till Powerbi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
   ![Power BI-instrumentpanel behörigheter](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

* <span data-ttu-id="e9916-135">Inloggning till PowerBI-webbplatsen och skapa instrumentpanel i realtid.</span><span class="sxs-lookup"><span data-stu-id="e9916-135">Login to PowerBI website, and create real-time dashboard.</span></span>

<span data-ttu-id="e9916-136">Nu är du redo att konfigurera Power BI-instrumentpanel med omfattande visualiseringar att få realtid och förutsägbara insikter om vehicle hälsa och köra vanor.</span><span class="sxs-lookup"><span data-stu-id="e9916-136">Now, you are ready to configure the Power BI dashboard with rich visualizations to gain real-time and predictive insights on vehicle health and driving habits.</span></span> <span data-ttu-id="e9916-137">Det tar cirka 45 minuter till en timme att skapa alla rapporter och konfigurera instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9916-137">It takes about 45 minutes to an hour to create all the reports and configure the dashboard.</span></span> 

### <a name="configure-power-bi-reports"></a><span data-ttu-id="e9916-138">Konfigurera Power BI-rapporter</span><span class="sxs-lookup"><span data-stu-id="e9916-138">Configure Power BI reports</span></span>
<span data-ttu-id="e9916-139">Realtid rapporter och instrumentpanelen ta ungefär 30-45 minuter för att slutföra.</span><span class="sxs-lookup"><span data-stu-id="e9916-139">The real-time reports and the dashboard take about 30-45 minutes to complete.</span></span> <span data-ttu-id="e9916-140">Bläddra till [http://powerbi.com](http://powerbi.com) och logga in.</span><span class="sxs-lookup"><span data-stu-id="e9916-140">Browse to [http://powerbi.com](http://powerbi.com) and login.</span></span>

![Logga in till Powerbi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

<span data-ttu-id="e9916-142">En ny datamängd skapas i Power BI.</span><span class="sxs-lookup"><span data-stu-id="e9916-142">A new dataset is generated in Power BI.</span></span> <span data-ttu-id="e9916-143">Klicka på den **ConnectedCarsRealtime** dataset.</span><span class="sxs-lookup"><span data-stu-id="e9916-143">Click the **ConnectedCarsRealtime** dataset.</span></span>

![Markerad anslutna bilar realtid dataset](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

<span data-ttu-id="e9916-145">Spara en tom rapport med hjälp av **Ctrl + s**.</span><span class="sxs-lookup"><span data-stu-id="e9916-145">Save the blank report using **Ctrl + s**.</span></span>

![Spara tom rapport](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

<span data-ttu-id="e9916-147">Ange rapportens namn *Vehicle telemetri Analytics realtid - rapporter*.</span><span class="sxs-lookup"><span data-stu-id="e9916-147">Provide report name *Vehicle Telemetry Analytics Real-time - Reports*.</span></span>

![Ange rapportens namn](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a><span data-ttu-id="e9916-149">Realtid rapporter</span><span class="sxs-lookup"><span data-stu-id="e9916-149">Real-time reports</span></span>
<span data-ttu-id="e9916-150">Det finns tre realtid rapporter i den här lösningen:</span><span class="sxs-lookup"><span data-stu-id="e9916-150">There are three real-time reports in this solution:</span></span>

1. <span data-ttu-id="e9916-151">Fordon i åtgärden</span><span class="sxs-lookup"><span data-stu-id="e9916-151">Vehicles in operation</span></span>
2. <span data-ttu-id="e9916-152">Fordon kräver Underhåll</span><span class="sxs-lookup"><span data-stu-id="e9916-152">Vehicles Requiring Maintenance</span></span>
3. <span data-ttu-id="e9916-153">Fordon hälsostatistik</span><span class="sxs-lookup"><span data-stu-id="e9916-153">Vehicles Health Statistics</span></span>

<span data-ttu-id="e9916-154">Du kan välja att konfigurera tre realtid rapporter eller stoppa efter någon gång och fortsätta till nästa avsnitt av batch-rapporter.</span><span class="sxs-lookup"><span data-stu-id="e9916-154">You can choose to configure all the three real-time reports or stop after any stage and proceed to the next section of configuring the batch reports.</span></span> <span data-ttu-id="e9916-155">Vi rekommenderar att du kan skapa tre rapporter att visualisera fullständig insikter i realtid sökvägen för lösningen.</span><span class="sxs-lookup"><span data-stu-id="e9916-155">We recommend you to create all the three reports to visualize the full insights of the real-time path of the solution.</span></span>  

### <a name="1-vehicles-in-operation"></a><span data-ttu-id="e9916-156">1. Fordon i åtgärden</span><span class="sxs-lookup"><span data-stu-id="e9916-156">1. Vehicles in operation</span></span>
<span data-ttu-id="e9916-157">Dubbelklicka på **sida 1** och Byt till ”fordon i åtgärden”</span><span class="sxs-lookup"><span data-stu-id="e9916-157">Double-click **Page 1** and rename it to “Vehicles in operation”</span></span>  
    <span data-ttu-id="e9916-158">![Anslutna bilar - fordon i åtgärd](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-158">![Connected Cars - Vehicles in operation](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)</span></span>  

<span data-ttu-id="e9916-159">Välj **vin** från **fält** och välj typ av visualiseringen som **”kort”**.</span><span class="sxs-lookup"><span data-stu-id="e9916-159">Select **vin** field from **Fields** and choose visualization type as **“Card”**.</span></span>  

<span data-ttu-id="e9916-160">Kort visualiseringen skapas som visas i bild.</span><span class="sxs-lookup"><span data-stu-id="e9916-160">Card visualization is created as shown in figure.</span></span>  
    ![Anslutna bilar - väljer vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

<span data-ttu-id="e9916-162">Klicka på det tomma utrymmet för att lägga till nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="e9916-162">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="e9916-163">Välj **Stad** och **vin** från fält.</span><span class="sxs-lookup"><span data-stu-id="e9916-163">Select **City** and **vin** from fields.</span></span> <span data-ttu-id="e9916-164">Ändra visualiseringen för **”karta”**.</span><span class="sxs-lookup"><span data-stu-id="e9916-164">Change visualization to **“Map”**.</span></span> <span data-ttu-id="e9916-165">Dra **vin** i värdeområdet.</span><span class="sxs-lookup"><span data-stu-id="e9916-165">Drag **vin** in values area.</span></span> <span data-ttu-id="e9916-166">Dra **Stad** från fält till **förklaring** område.</span><span class="sxs-lookup"><span data-stu-id="e9916-166">Drag **city** from fields to **Legend** area.</span></span>   
    <span data-ttu-id="e9916-167">![Ansluten bilar - kort visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-167">![Connected Cars - Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)</span></span>

<span data-ttu-id="e9916-168">Välj **format** avsnittet från **visualiseringar**, klickar du på **rubrik** och ändra den **Text** till **”fordon i åtgärd efter ort”**.</span><span class="sxs-lookup"><span data-stu-id="e9916-168">Select **format** section from **Visualizations**, click **Title** and change the **Text** to **“Vehicles in operation by city”**.</span></span>  
    <span data-ttu-id="e9916-169">![Anslutna bilar - fordon i åtgärd efter ort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-169">![Connected Cars - Vehicles in operation by city](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)</span></span>   

<span data-ttu-id="e9916-170">Sista visualiseringen ser ut som visas i bild.</span><span class="sxs-lookup"><span data-stu-id="e9916-170">Final visualization looks as shown in figure.</span></span>    
    ![Anslutna bilar - slutliga visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

<span data-ttu-id="e9916-172">Klicka på det tomma utrymmet för att lägga till nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="e9916-172">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="e9916-173">Välj **Stad** och **vin**, ändra typen av visualisering till **grupperat stående stapeldiagram**.</span><span class="sxs-lookup"><span data-stu-id="e9916-173">Select **City** and **vin**, change visualization type to **Clustered Column Chart**.</span></span> <span data-ttu-id="e9916-174">Se till att **Stad** i **axel området** och **vin** i **värdet område**</span><span class="sxs-lookup"><span data-stu-id="e9916-174">Ensure **City** field in **Axis area** and **vin** in **Value area**</span></span>  

<span data-ttu-id="e9916-175">Sortera diagram av **”antal vin”**</span><span class="sxs-lookup"><span data-stu-id="e9916-175">Sort chart by **“Count of vin”**</span></span>  
    <span data-ttu-id="e9916-176">![Anslutna bilar - antal vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-176">![Connected Cars - Count of vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)</span></span>  

<span data-ttu-id="e9916-177">Ändra diagramtyp **rubrik** till **”fordon i åtgärd efter ort”**</span><span class="sxs-lookup"><span data-stu-id="e9916-177">Change chart **Title** to **“Vehicles in operation by city”**</span></span>  

<span data-ttu-id="e9916-178">Klicka på den **Format** avsnittet och väljer sedan **Data färger**, klickar du på den **”på”** till **visa alla**</span><span class="sxs-lookup"><span data-stu-id="e9916-178">Click the **Format** section, then select **Data Colors**,  Click the **“On”** to **Show All**</span></span>  
    <span data-ttu-id="e9916-179">![Anslutna bilar – visa alla Data färger](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-179">![Connected Cars - Show all Data Colors](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)</span></span>  

<span data-ttu-id="e9916-180">Ändra färgen på enskilda ort genom att klicka på ikonen färg.</span><span class="sxs-lookup"><span data-stu-id="e9916-180">Change the color of individual city by clicking on color icon.</span></span>  
    ![Ansluten bilar - ändra färger](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

<span data-ttu-id="e9916-182">Klicka på det tomma utrymmet för att lägga till nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="e9916-182">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="e9916-183">Välj **grupperat stående stapeldiagram** visualiseringen från visualiseringar, dra **Stad** i **axel** området **modellen** i **förklaring** området och **vin** i **värdet** område.</span><span class="sxs-lookup"><span data-stu-id="e9916-183">Select **Clustered Column Chart** visualization from visualizations, drag **city** field in **Axis** area, **Model** in **Legend** area and **vin** in **Value** area.</span></span>  
    <span data-ttu-id="e9916-184">![Anslutna bilar - grupperat stående stapeldiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-184">![Connected Cars - Clustered Column Chart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)</span></span>  
    <span data-ttu-id="e9916-185">![Anslutna bilar - återgivning](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-185">![Connected Cars - Rendering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)</span></span>

<span data-ttu-id="e9916-186">Ordna om alla visualiseringen på den här sidan som visas i bild.</span><span class="sxs-lookup"><span data-stu-id="e9916-186">Rearrange all visualization on this page as shown in figure.</span></span>  
    ![Anslutna bilar - visualiseringar](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

<span data-ttu-id="e9916-188">Du har konfigurerat ”fordon i åtgärden” realtid rapporten.</span><span class="sxs-lookup"><span data-stu-id="e9916-188">You have successfully configured the “Vehicles in operation” real-time report.</span></span> <span data-ttu-id="e9916-189">Du kan fortsätta att skapa nästa realtid rapport eller stoppa här och konfigurera instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9916-189">You can proceed to create the next real-time report or stop here and configure the dashboard.</span></span> 

### <a name="2-vehicles-requiring-maintenance"></a><span data-ttu-id="e9916-190">2. Fordon kräver Underhåll</span><span class="sxs-lookup"><span data-stu-id="e9916-190">2. Vehicles Requiring Maintenance</span></span>
<span data-ttu-id="e9916-191">Klicka på ![Lägg till](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) om du vill lägga till en ny rapport, byta namn på den till **”fordon kräver Underhåll”**</span><span class="sxs-lookup"><span data-stu-id="e9916-191">Click ![Add](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) to add a new report, rename it to **“Vehicles Requiring Maintenance”**</span></span>

![Anslutna bilar - fordon kräver Underhåll](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

<span data-ttu-id="e9916-193">Välj **vin** fältet och ändra typen av visualisering till **kort**.</span><span class="sxs-lookup"><span data-stu-id="e9916-193">Select **vin** field and change visualization type to **Card**.</span></span>  
    <span data-ttu-id="e9916-194">![Anslutna bilar - Vin kort visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-194">![Connected Cars - Vin Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)</span></span>  

<span data-ttu-id="e9916-195">Vi har ett fält med namnet ”MaintenanceLabel” i datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="e9916-195">We have a field named “MaintenanceLabel” In the dataset.</span></span> <span data-ttu-id="e9916-196">Det här fältet kan ha värdet ”0” eller ”1” ”.</span><span class="sxs-lookup"><span data-stu-id="e9916-196">This field can have a value of “0” or “1”.”</span></span> <span data-ttu-id="e9916-197">Den anges av Azure Machine Learning-modell etablerats som en del av lösningen och integrerats med realtid sökvägen.</span><span class="sxs-lookup"><span data-stu-id="e9916-197">It is set by the Azure Machine Learning model provisioned as part of solution and integrated with the real-time path.</span></span> <span data-ttu-id="e9916-198">Värdet ”1” anger fordon kräver underhåll.</span><span class="sxs-lookup"><span data-stu-id="e9916-198">The value “1” indicates a vehicle requires maintenance.</span></span> 

<span data-ttu-id="e9916-199">Att lägga till en **sidnivå** filter för att visa fordon data, vilket kräver Underhåll:</span><span class="sxs-lookup"><span data-stu-id="e9916-199">To add a **Page Level** filter for showing vehicles data, which are requiring maintenance:</span></span> 

1. <span data-ttu-id="e9916-200">Dra den **”MaintenanceLabel”** omvandlas **nivå sidfilter**.</span><span class="sxs-lookup"><span data-stu-id="e9916-200">Drag the **“MaintenanceLabel”** field into **Page Level Filters**.</span></span>  
   <span data-ttu-id="e9916-201">![Anslutna bilar - sidnivå filter](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-201">![Connected Cars - Page Level Filters](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)</span></span>  
2. <span data-ttu-id="e9916-202">Klicka på **grundläggande filtrering** menyn som finns längst ned i MaintenanceLabel sidfilter nivå.</span><span class="sxs-lookup"><span data-stu-id="e9916-202">Click **Basic Filtering** menu present at bottom of MaintenanceLabel Page Level Filter.</span></span>  
   <span data-ttu-id="e9916-203">![Ansluten bilar - grundläggande filtrering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-203">![Connected Cars - Basic Filtering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)</span></span>  
3. <span data-ttu-id="e9916-204">Ange filtervärdet **”1”**  </span><span class="sxs-lookup"><span data-stu-id="e9916-204">Set its filter value to **“1”**  </span></span>  
   <span data-ttu-id="e9916-205">![Anslutna bilar - filtervärdet](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-205">![Connected Cars - Filter Value](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)</span></span>  

<span data-ttu-id="e9916-206">Klicka på det tomma utrymmet för att lägga till nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="e9916-206">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="e9916-207">Välj **grupperat stående stapeldiagram** från visualiseringar</span><span class="sxs-lookup"><span data-stu-id="e9916-207">Select **Clustered Column Chart** from visualizations</span></span>  
<span data-ttu-id="e9916-208">![Anslutna bilar - Vind kort visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-208">![Connected Cars - Vind Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)</span></span>  
<span data-ttu-id="e9916-209">![Anslutna bilar - grupperat stående stapeldiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-209">![Connected Cars - Clustered Column Chart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)</span></span>

<span data-ttu-id="e9916-210">Dra fältet **modellen** till **axel** området **Vin** till **värdet** område.</span><span class="sxs-lookup"><span data-stu-id="e9916-210">Drag field **Model** into **Axis** area, **Vin** to **Value** area.</span></span> <span data-ttu-id="e9916-211">Sortera visualisering av **antal vin**.</span><span class="sxs-lookup"><span data-stu-id="e9916-211">Then sort visualization by **Count of vin**.</span></span>  <span data-ttu-id="e9916-212">Ändra diagramtyp **rubrik** till **”fordon kräver underhåll av modell”**</span><span class="sxs-lookup"><span data-stu-id="e9916-212">Change chart **Title** to **“Vehicles requiring maintenance by model”**</span></span>  

<span data-ttu-id="e9916-213">Dra **vin** fält i **färgmättnad** finns på **fält** ![fält](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) avsnitt i **visualiseringen** fliken</span><span class="sxs-lookup"><span data-stu-id="e9916-213">Drag **vin** fields into **Color Saturation** present at **Fields** ![Fields](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) section of **Visualization** tab</span></span>  
<span data-ttu-id="e9916-214">![Anslutna bilar - färgmättnad](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-214">![Connected Cars - Color Saturation](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)</span></span>  

<span data-ttu-id="e9916-215">Ändra **Data färger** i visualiseringar från **Format** avsnitt</span><span class="sxs-lookup"><span data-stu-id="e9916-215">Change **Data Colors** in visualizations from **Format** section</span></span>  
<span data-ttu-id="e9916-216">Ändra minimifärg till: **F2C812**</span><span class="sxs-lookup"><span data-stu-id="e9916-216">Change Minimum color to: **F2C812**</span></span>  
<span data-ttu-id="e9916-217">Ändra maxfärg till: **FF6300**</span><span class="sxs-lookup"><span data-stu-id="e9916-217">Change Maximum color to: **FF6300**</span></span>  
<span data-ttu-id="e9916-218">![Ansluten bilar - ändringar](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-218">![Connected Cars - Color Changes](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)</span></span>  
<span data-ttu-id="e9916-219">![Ansluten bilar - nya Visualiseringsfärger](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-219">![Connected Cars - New Visualization Colors](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)</span></span>  

<span data-ttu-id="e9916-220">Klicka på det tomma utrymmet för att lägga till nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="e9916-220">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="e9916-221">Välj **klustrade stapeldiagram** dra från visualiseringar, **vin** fältet i **värdet** område, dra **Stad** omvandlas **axel** område.</span><span class="sxs-lookup"><span data-stu-id="e9916-221">Select **Clustered column chart** from visualizations, drag **vin** field into **Value** area, drag **City** field into **Axis** area.</span></span> <span data-ttu-id="e9916-222">Sortera diagram av **”antal vin”**.</span><span class="sxs-lookup"><span data-stu-id="e9916-222">Sort chart by **“Count of vin”**.</span></span> <span data-ttu-id="e9916-223">Ändra diagramtyp **rubrik** till **”fordon kräver underhåll av ort”** </span><span class="sxs-lookup"><span data-stu-id="e9916-223">Change chart **Title** to **“Vehicles requiring maintenance by city”** </span></span>  
<span data-ttu-id="e9916-224">![Anslutna bilar - fordon kräver underhåll av ort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-224">![Connected Cars - Vehicles requiring maintenance by city](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)</span></span>  

<span data-ttu-id="e9916-225">Klicka på det tomma utrymmet för att lägga till nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="e9916-225">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="e9916-226">Välj **flerradiga kort** visualiseringen från visualiseringar, dra **modellen** och **vin** till den **fält** område.</span><span class="sxs-lookup"><span data-stu-id="e9916-226">Select **Multi-Row Card** visualization from visualizations, drag **Model** and **vin** into the **Fields** area.</span></span>  
<span data-ttu-id="e9916-227">![Anslutna bilar - flerradiga-kort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-227">![Connected Cars - Multi-Row Card](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)</span></span>    

<span data-ttu-id="e9916-228">Flytta alla visualiseringen den slutgiltiga rapporten ser ut som följer:</span><span class="sxs-lookup"><span data-stu-id="e9916-228">Rearranging all of the visualization, the final report looks as follows:</span></span>  
![Anslutna bilar - flerradiga-kort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

<span data-ttu-id="e9916-230">Du har konfigurerat rapporten ”fordon kräver Underhåll” realtid.</span><span class="sxs-lookup"><span data-stu-id="e9916-230">You have successfully configured the “Vehicles Requiring Maintenance” real-time report.</span></span> <span data-ttu-id="e9916-231">Du kan fortsätta att skapa nästa realtid rapport eller stoppa här och konfigurera instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9916-231">You can proceed to create the next real-time report or stop here and configure the dashboard.</span></span> 

### <a name="3-vehicles-health-statistics"></a><span data-ttu-id="e9916-232">3. Fordon hälsostatistik</span><span class="sxs-lookup"><span data-stu-id="e9916-232">3. Vehicles Health Statistics</span></span>
<span data-ttu-id="e9916-233">Klicka på ![Lägg till](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) om du vill lägga till ny rapport, byta namn på den till **”fordon hälsostatistik”**</span><span class="sxs-lookup"><span data-stu-id="e9916-233">Click ![Add](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) to add new report, rename it to **“Vehicles Health Statistics”**</span></span>  

<span data-ttu-id="e9916-234">Välj **mätaren** visualiseringen från visualiseringar, drar den **hastighet** omvandlas **värde, minimalt värde maxvärdet** områden.</span><span class="sxs-lookup"><span data-stu-id="e9916-234">Select **Gauge** visualization from visualizations, then drag the **Speed** field into **Value, Minimum Value, Maximum Value** areas.</span></span>  
<span data-ttu-id="e9916-235">![Anslutna bilar - flerradiga-kort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)</span><span class="sxs-lookup"><span data-stu-id="e9916-235">![Connected Cars - Multi-Row Card](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)</span></span>  

<span data-ttu-id="e9916-236">Ändra standard sammanställning **hastighet** i **värdet området** till **Genomsnittlig**</span><span class="sxs-lookup"><span data-stu-id="e9916-236">Change the default aggregation of **speed** in **Value area** to **Average**</span></span> 

<span data-ttu-id="e9916-237">Ändra standard sammanställning **hastighet** i **minsta område** till **minsta**</span><span class="sxs-lookup"><span data-stu-id="e9916-237">Change the default aggregation of **speed** in **Minimum area** to **Minimum**</span></span>

<span data-ttu-id="e9916-238">Ändra standard sammanställning **hastighet** i **maximalt området** till **maximala**</span><span class="sxs-lookup"><span data-stu-id="e9916-238">Change the default aggregation of **speed** in **Maximum area** to **Maximum**</span></span>

![Anslutna bilar - flerradiga-kort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

<span data-ttu-id="e9916-240">Byt namn på den **mätaren rubrik** till **”genomsnittlig hastighet”**</span><span class="sxs-lookup"><span data-stu-id="e9916-240">Rename the **Gauge Title** to **“Average speed”**</span></span> 

![Anslutna bilar - mätare](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

<span data-ttu-id="e9916-242">Klicka på det tomma utrymmet för att lägga till nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="e9916-242">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="e9916-243">Lägga till en **mätaren** för **genomsnittlig motorolja**, **genomsnittlig bränsle**, och **genomsnittlig motorn tempererade**.</span><span class="sxs-lookup"><span data-stu-id="e9916-243">Similarly add a **Gauge** for **average engine oil**, **average fuel**, and **average engine temperate**.</span></span>  

<span data-ttu-id="e9916-244">Ändra standard aggregering av fälten i varje mätaren enligt ovan stegen i **”genomsnittlig hastighet”** mätare.</span><span class="sxs-lookup"><span data-stu-id="e9916-244">Change the default aggregation of fields in each gauge as per above steps in **“Average speed”** gauge.</span></span>

![Anslutna bilar - mätare](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

<span data-ttu-id="e9916-246">Klicka på det tomma utrymmet för att lägga till nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="e9916-246">Click the blank area to add new visualization.</span></span>

<span data-ttu-id="e9916-247">Välj **linjediagram och grupperat stående stapeldiagram** från visualiseringar, dra **Stad** omvandlas **delade axel**, dra **hastighet**, **fälten tirepressure och engineoil** till **kolumnvärdena** området ändra sina sammansättningstyp till **genomsnittlig**.</span><span class="sxs-lookup"><span data-stu-id="e9916-247">Select **Line and Clustered Column Chart** from visualizations, then drag **City** field into **Shared Axis**, drag **speed**, **tirepressure and engineoil fields** into **Column Values** area, change their aggregation type to **Average**.</span></span> 

<span data-ttu-id="e9916-248">Dra den **engineTemperature** omvandlas **radvärden** område, ändra Aggregeringstyp till **genomsnittlig**.</span><span class="sxs-lookup"><span data-stu-id="e9916-248">Drag the **engineTemperature** field into **Line Values** area, change the  aggregation type to **Average**.</span></span> 

![Anslutna bilar - visualiseringar fält](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

<span data-ttu-id="e9916-250">Ändra diagrammet **rubrik** till **”genomsnittlig hastighet, däck tryck, motorolja och motorn temperatur”**.</span><span class="sxs-lookup"><span data-stu-id="e9916-250">Change the chart **Title** to **“Average speed, tire pressure, engine oil and engine temperature”**.</span></span>  

![Anslutna bilar - visualiseringar fält](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

<span data-ttu-id="e9916-252">Klicka på det tomma utrymmet för att lägga till nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="e9916-252">Click the blank area to add new visualization.</span></span>

<span data-ttu-id="e9916-253">Välj **Treemap** visualiseringen från visualiseringar, drar den **modellen** omvandlas den **grupp** området och dra fältet **MaintenanceProbability** i den **värden** område.</span><span class="sxs-lookup"><span data-stu-id="e9916-253">Select **Treemap** visualization from visualizations, drag the **Model** field into the **Group** area, and drag the field **MaintenanceProbability** into the **Values** area.</span></span>

<span data-ttu-id="e9916-254">Ändra diagrammet **rubrik** till **”Vehicle modeller som kräver Underhåll”**.</span><span class="sxs-lookup"><span data-stu-id="e9916-254">Change the chart **Title** to **“Vehicle models requiring maintenance”**.</span></span>

![Ansluten bilar - ändra diagrammets rubrik](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

<span data-ttu-id="e9916-256">Klicka på det tomma utrymmet för att lägga till nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="e9916-256">Click the blank area to add new visualization.</span></span>

<span data-ttu-id="e9916-257">Välj **100% staplad liggande diagram** dra från visualisering, den **Stad** omvandlas den **axel** området och dra den **MaintenanceProbability**, **RecallProbability** fält i den **värdet** område.</span><span class="sxs-lookup"><span data-stu-id="e9916-257">Select **100% Stacked Bar Chart** from visualization, drag the **city** field into the **Axis** area, and drag the **MaintenanceProbability**, **RecallProbability** fields into the **Value** area.</span></span>

![Anslutna bilar - Lägg till nya visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

<span data-ttu-id="e9916-259">Klicka på **Format**väljer **Data färger**, och ange den **MaintenanceProbability** färg till värdet **”F2C80F”**.</span><span class="sxs-lookup"><span data-stu-id="e9916-259">Click **Format**, select **Data Colors**, and set the **MaintenanceProbability** color to the value **“F2C80F”**.</span></span>

<span data-ttu-id="e9916-260">Ändra den **rubrik** diagrammet genom att **”sannolikheten för Vehicle underhåll och återkalla av ort”**.</span><span class="sxs-lookup"><span data-stu-id="e9916-260">Change the **Title** of the chart to **“Probability of Vehicle Maintenance & Recall by City”**.</span></span>

![Anslutna bilar - Lägg till nya visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

<span data-ttu-id="e9916-262">Klicka på det tomma utrymmet för att lägga till nya visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="e9916-262">Click the blank area to add new visualization.</span></span>

<span data-ttu-id="e9916-263">Välj **ytdiagram** dra från visualiseringen från visualiseringar i **modellen** omvandlas den **axel** området och dra den **engineOil tirepressure, hastighet och MaintenanceProbability** fält i den **värden** område.</span><span class="sxs-lookup"><span data-stu-id="e9916-263">Select **Area Chart** from visualization from visualizations, drag the **Model** field into the **Axis** area, and drag the **engineOil, tirepressure, speed and MaintenanceProbability** fields into the **Values** area.</span></span> <span data-ttu-id="e9916-264">Ändra sina sammansättningstyp till **”genomsnittliga”**.</span><span class="sxs-lookup"><span data-stu-id="e9916-264">Change their aggregation type to **“Average”**.</span></span> 

![Ansluten bilar - ändra sammansättningstyp](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

<span data-ttu-id="e9916-266">Ändra titeln på diagrammet på **”genomsnittlig motorolja, tröttnar sannolikheten för hög belastning, hastighet och underhåll av modell”**.</span><span class="sxs-lookup"><span data-stu-id="e9916-266">Change the title of the chart to **“Average engine oil, tire pressure, speed and maintenance probability by model”**.</span></span>

![Ansluten bilar - ändra diagrammets rubrik](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

<span data-ttu-id="e9916-268">Klicka på det tomma utrymmet för att lägga till nya visualiseringen:</span><span class="sxs-lookup"><span data-stu-id="e9916-268">Click the blank area to add new visualization:</span></span>

1. <span data-ttu-id="e9916-269">Välj **punktdiagram** visualiseringen från visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="e9916-269">Select **Scatter Chart** visualization from visualizations.</span></span>
2. <span data-ttu-id="e9916-270">Dra den **modellen** omvandlas den **information** och **förklaring** område.</span><span class="sxs-lookup"><span data-stu-id="e9916-270">Drag the **Model** field into the **Details** and **Legend** area.</span></span>
3. <span data-ttu-id="e9916-271">Dra den **bränsle** omvandlas den **x-axeln** område, ändra aggregering till **genomsnittlig**.</span><span class="sxs-lookup"><span data-stu-id="e9916-271">Drag the **fuel** field into the **X-Axis** area, change the aggregation to **Average**.</span></span>
4. <span data-ttu-id="e9916-272">Dra **engineTemparature** till **y-axeln området**, ändra aggregering till **Genomsnittlig**</span><span class="sxs-lookup"><span data-stu-id="e9916-272">Drag **engineTemparature** into **Y-Axis area**, change the aggregation to **Average**</span></span>
5. <span data-ttu-id="e9916-273">Dra den **vin** omvandlas den **storlek** område.</span><span class="sxs-lookup"><span data-stu-id="e9916-273">Drag the **vin** field into the **Size** area.</span></span>

![Anslutna bilar - Lägg till nya visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

<span data-ttu-id="e9916-275">Ändra diagrammet **rubrik** till **”medelvärden bränsle, motorn temperatur av modell”**.</span><span class="sxs-lookup"><span data-stu-id="e9916-275">Change the chart **Title** to **“Averages of Fuel, Engine Temperature by Model”**.</span></span>

![Ansluten bilar - ändra diagrammets rubrik](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

<span data-ttu-id="e9916-277">Den slutliga rapporten ser ut som nedan.</span><span class="sxs-lookup"><span data-stu-id="e9916-277">The final report will look like as shown below.</span></span>

![Ansluten bilar-slutgiltig rapport](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a><span data-ttu-id="e9916-279">PIN-kod visualiseringar från rapporterna i realtid instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="e9916-279">Pin visualizations from the reports to the real-time dashboard</span></span>
<span data-ttu-id="e9916-280">Skapa en tom instrumentpanel genom att klicka på plusikonen bredvid instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="e9916-280">Create a blank dashboard by clicking on the plus icon next to Dashboards.</span></span> <span data-ttu-id="e9916-281">Du kan kalla den ”Vehicle telemetri instrumentpanelen”</span><span class="sxs-lookup"><span data-stu-id="e9916-281">You can name it “Vehicle Telemetry Analytics Dashboard”</span></span>

![Anslutna bilar-instrumentpanelen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

<span data-ttu-id="e9916-283">Fäst visualiseringen från ovan rapporterna på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9916-283">Pin the visualization from the above reports to the dashboard.</span></span> 

![Anslutna bilar-instrumentpanelen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

<span data-ttu-id="e9916-285">Instrumentpanelen ska se ut så här när alla tre rapporter skapas och motsvarande visualiseringar är fäst på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9916-285">The dashboard should look as follows when all the three reports are created and the corresponding visualizations are pinned to the dashboard.</span></span> <span data-ttu-id="e9916-286">Om du inte har skapat alla rapporter kan instrumentpanelen se annorlunda ut.</span><span class="sxs-lookup"><span data-stu-id="e9916-286">If you have not created all the reports, your dashboard could look different.</span></span> 

![Anslutna bilar-instrumentpanelen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

<span data-ttu-id="e9916-288">Grattis!</span><span class="sxs-lookup"><span data-stu-id="e9916-288">Congratulations!</span></span> <span data-ttu-id="e9916-289">Du har skapat realtid instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9916-289">You have successfully created the real-time dashboard.</span></span> <span data-ttu-id="e9916-290">När du fortsätter att köra CarEventGenerator.exe och RealtimeDashboardApp.exe bör du se live uppdateringar på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9916-290">As you continue to execute CarEventGenerator.exe and RealtimeDashboardApp.exe, you should see live updates on the dashboard.</span></span> <span data-ttu-id="e9916-291">Det bör ta ungefär 10 – 15 minuter för att slutföra följande steg.</span><span class="sxs-lookup"><span data-stu-id="e9916-291">It should take about 10 to 15 minutes to complete the following steps.</span></span>

## <a name="setup-power-bi-batch-processing-dashboard"></a><span data-ttu-id="e9916-292">Installera Power BI batch-bearbetning instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="e9916-292">Setup Power BI batch processing dashboard</span></span>
> [!NOTE]
> <span data-ttu-id="e9916-293">Det tar ca två timmar (från distributionen lyckades) för slutpunkt till slutpunkt batchbearbetning pipelinen Slutför körning och bearbetar ett års värt att skapas.</span><span class="sxs-lookup"><span data-stu-id="e9916-293">It takes about two hours (from the successful completion of the deployment) for the end to end batch processing pipeline to finish execution and process a year worth of generated data.</span></span> <span data-ttu-id="e9916-294">Så vänta tills bearbetningen ska slutföras innan du fortsätter med nästa steg.</span><span class="sxs-lookup"><span data-stu-id="e9916-294">So wait for the processing to finish before proceeding with the next steps.</span></span> 
> 
> 

<span data-ttu-id="e9916-295">**Hämta filen Power BI designer**</span><span class="sxs-lookup"><span data-stu-id="e9916-295">**Download the Power BI designer file**</span></span>

* <span data-ttu-id="e9916-296">En förkonfigurerad Power BI designer fil ingår som en del av distributionen manuell åtgärd instruktioner</span><span class="sxs-lookup"><span data-stu-id="e9916-296">A pre-configured Power BI designer file is included as part of the deployment Manual Operation Instructions</span></span>
* <span data-ttu-id="e9916-297">Leta efter 2.</span><span class="sxs-lookup"><span data-stu-id="e9916-297">Look for 2.</span></span> <span data-ttu-id="e9916-298">Installationsprogrammet PowerBI batch bearbetning instrumentpanelen som du kan hämta PowerBI-mallen för batchbearbetning instrumentpanelen namnet **ConnectedCarsPbiReport.pbix**.</span><span class="sxs-lookup"><span data-stu-id="e9916-298">Setup PowerBI batch processing dashboard You can download the PowerBI template for batch processing dashboard here called **ConnectedCarsPbiReport.pbix**.</span></span>
* <span data-ttu-id="e9916-299">Spara lokalt</span><span class="sxs-lookup"><span data-stu-id="e9916-299">Save locally</span></span>

<span data-ttu-id="e9916-300">**Konfigurera Power BI-rapporter**</span><span class="sxs-lookup"><span data-stu-id="e9916-300">**Configure Power BI reports**</span></span>

* <span data-ttu-id="e9916-301">Öppna filen designer '**ConnectedCarsPbiReport.pbix**' med hjälp av Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="e9916-301">Open the designer file ‘**ConnectedCarsPbiReport.pbix**’ using Power BI Desktop.</span></span> <span data-ttu-id="e9916-302">Om du inte redan har, installera Power BI Desktop från [Power BI Desktop installera](http://www.microsoft.com/download/details.aspx?id=45331).</span><span class="sxs-lookup"><span data-stu-id="e9916-302">If you do not already have, install the Power BI Desktop from [Power BI Desktop install](http://www.microsoft.com/download/details.aspx?id=45331).</span></span> 
* <span data-ttu-id="e9916-303">Klicka på den **redigera frågor**.</span><span class="sxs-lookup"><span data-stu-id="e9916-303">Click the **Edit Queries**.</span></span>

![Redigera Power BI-fråga](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

* <span data-ttu-id="e9916-305">Dubbelklicka på den **källa**.</span><span class="sxs-lookup"><span data-stu-id="e9916-305">Double-click the **Source**.</span></span>

![Ange Power BI-källa](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

* <span data-ttu-id="e9916-307">Uppdatera Server-anslutningssträngen med Azure SQL-server som har etablerats som en del av distributionen.</span><span class="sxs-lookup"><span data-stu-id="e9916-307">Update Server connection string with the Azure SQL server that got provisioned as part of the deployment.</span></span>  <span data-ttu-id="e9916-308">Leta i anvisningarna under manuell åtgärd</span><span class="sxs-lookup"><span data-stu-id="e9916-308">Look in the Manual Operation Instructions under</span></span> 

    4. <span data-ttu-id="e9916-309">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="e9916-309">Azure SQL Database</span></span>
    
    * <span data-ttu-id="e9916-310">Server: somethingsrv.database.windows.net</span><span class="sxs-lookup"><span data-stu-id="e9916-310">Server: somethingsrv.database.windows.net</span></span>
    * <span data-ttu-id="e9916-311">Databas: connectedcar</span><span class="sxs-lookup"><span data-stu-id="e9916-311">Database: connectedcar</span></span>
    * <span data-ttu-id="e9916-312">Användarnamn: användarnamn</span><span class="sxs-lookup"><span data-stu-id="e9916-312">Username: username</span></span>
    * <span data-ttu-id="e9916-313">Lösenord: Du kan hantera ditt lösenord för SQL server från Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e9916-313">Password: You can manage your SQL server password from Azure portal</span></span>

* <span data-ttu-id="e9916-314">Lämna **databasen** som *connectedcar*.</span><span class="sxs-lookup"><span data-stu-id="e9916-314">Leave **Database** as *connectedcar*.</span></span>

![Ange Power BI-databasen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

* <span data-ttu-id="e9916-316">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e9916-316">Click **OK**.</span></span>
* <span data-ttu-id="e9916-317">Du ser **Windows-autentiseringsuppgifter** fliken markerad som standard kan ändra det till **databasen autentiseringsuppgifter** genom att klicka på **databasen** fliken längst till höger.</span><span class="sxs-lookup"><span data-stu-id="e9916-317">You will see **Windows credential** tab selected by default, change it to **Database credentials** by clicking on **Database** tab at right.</span></span>
* <span data-ttu-id="e9916-318">Ange den **användarnamn** och **lösenord** i din Azure SQL Database som angavs under installationen för distribution.</span><span class="sxs-lookup"><span data-stu-id="e9916-318">Provide the **Username** and **Password** of your Azure SQL Database that was specified during its deployment setup.</span></span>

![Ange Databasautentiseringsuppgifter för](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

* <span data-ttu-id="e9916-320">Klicka på **ansluta**</span><span class="sxs-lookup"><span data-stu-id="e9916-320">Click **Connect**</span></span>
* <span data-ttu-id="e9916-321">Upprepa stegen ovan för alla tre återstående frågor finns i högra fönstret och uppdatera information om datakälla anslutning.</span><span class="sxs-lookup"><span data-stu-id="e9916-321">Repeat the above steps for each of the three remaining queries present at right pane, and then update the data source connection details.</span></span>
* <span data-ttu-id="e9916-322">Klicka på **Stäng och läsa in**.</span><span class="sxs-lookup"><span data-stu-id="e9916-322">Click **Close and Load**.</span></span> <span data-ttu-id="e9916-323">Power BI Desktop filen datauppsättningar ansluts till SQL Azure databastabeller.</span><span class="sxs-lookup"><span data-stu-id="e9916-323">Power BI Desktop file datasets are connected to SQL Azure Database tables.</span></span>
* <span data-ttu-id="e9916-324">**Stäng** Power BI Desktop-fil.</span><span class="sxs-lookup"><span data-stu-id="e9916-324">**Close** Power BI Desktop file.</span></span>

![Stäng Power BI desktop](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

* <span data-ttu-id="e9916-326">Klicka på **spara** för att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="e9916-326">Click **Save** button to save the changes.</span></span> 

<span data-ttu-id="e9916-327">Du har nu konfigurerat alla rapporter som motsvarar sökvägen för batch-bearbetning i lösningen.</span><span class="sxs-lookup"><span data-stu-id="e9916-327">You have now configured all the reports corresponding to the batch processing path in the solution.</span></span> 

## <a name="upload-to-powerbicom"></a><span data-ttu-id="e9916-328">Överför till *powerbi.com*</span><span class="sxs-lookup"><span data-stu-id="e9916-328">Upload to *powerbi.com*</span></span>
1. <span data-ttu-id="e9916-329">Gå till Power BI-webbportalen på http://powerbi.com och logga in.</span><span class="sxs-lookup"><span data-stu-id="e9916-329">Navigate to the Power BI web portal at http://powerbi.com and login.</span></span>
2. <span data-ttu-id="e9916-330">Klicka på **hämta Data**</span><span class="sxs-lookup"><span data-stu-id="e9916-330">Click **Get Data**</span></span>  
3. <span data-ttu-id="e9916-331">Ladda upp filen Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="e9916-331">Upload the Power BI Desktop File.</span></span>  
4. <span data-ttu-id="e9916-332">Om du vill ladda upp, klickar du på **hämta Data -> hämta filer -> lokal fil**</span><span class="sxs-lookup"><span data-stu-id="e9916-332">To upload, click **Get Data -> Files Get -> Local file**</span></span>  
5. <span data-ttu-id="e9916-333">Navigera till den **”**ConnectedCarsPbiReport.pbix**”**</span><span class="sxs-lookup"><span data-stu-id="e9916-333">Navigate to the **“**ConnectedCarsPbiReport.pbix**”**</span></span>  
6. <span data-ttu-id="e9916-334">När filen har överförts dirigeras till arbetsplatsen Power BI.</span><span class="sxs-lookup"><span data-stu-id="e9916-334">Once the file is uploaded, you will be navigated back to your Power BI work space.</span></span>  

<span data-ttu-id="e9916-335">En datamängd, rapporten och en tom instrumentpanel skapas för dig.</span><span class="sxs-lookup"><span data-stu-id="e9916-335">A dataset, report and a blank dashboard will be created for you.</span></span>  

<span data-ttu-id="e9916-336">Diagram för PIN-kod till en ny instrumentpanel kallas **Vehicle telemetri instrumentpanelen** i **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="e9916-336">Pin charts to a new dashboard called **Vehicle Telemetry Analytics Dashboard** in **Power BI**.</span></span> <span data-ttu-id="e9916-337">Klicka på den tomma instrumentpanelen skapade ovan och gå sedan till den **rapporter** avsnitt på rapporten nyligen uppladdade.</span><span class="sxs-lookup"><span data-stu-id="e9916-337">Click the blank dashboard created above and then navigate to the **Reports** section click the newly uploaded report.</span></span>  

![Vehicle telemetri Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

<span data-ttu-id="e9916-339">**Observera att rapporten har sex sidor:**</span><span class="sxs-lookup"><span data-stu-id="e9916-339">**Note the report has six pages:**</span></span>  
<span data-ttu-id="e9916-340">Sidan 1: Vehicle densitet</span><span class="sxs-lookup"><span data-stu-id="e9916-340">Page 1: Vehicle density</span></span>  
<span data-ttu-id="e9916-341">Sidan 2: Realtid vehicle hälsa</span><span class="sxs-lookup"><span data-stu-id="e9916-341">Page 2: Real-time vehicle health</span></span>  
<span data-ttu-id="e9916-342">Sidan 3: Aggressivt drivs fordon</span><span class="sxs-lookup"><span data-stu-id="e9916-342">Page 3: Aggressively Driven Vehicles</span></span>   
<span data-ttu-id="e9916-343">Sidan 4: Återkallas fordon</span><span class="sxs-lookup"><span data-stu-id="e9916-343">Page 4: Recalled vehicles</span></span>  
<span data-ttu-id="e9916-344">Sidan 5: Bränsle effektivt drivs fordon</span><span class="sxs-lookup"><span data-stu-id="e9916-344">Page 5: Fuel Efficiently Driven Vehicles</span></span>  
<span data-ttu-id="e9916-345">Sidan 6: Contoso-logotyp</span><span class="sxs-lookup"><span data-stu-id="e9916-345">Page 6: Contoso Logo</span></span>  

![Anslutna bilar Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

<span data-ttu-id="e9916-347">**Från sidan 3**, fästa följande:</span><span class="sxs-lookup"><span data-stu-id="e9916-347">**From Page 3**, pin the following:</span></span>  

1. <span data-ttu-id="e9916-348">Antal VIN</span><span class="sxs-lookup"><span data-stu-id="e9916-348">Count of VIN</span></span>  
   ![Anslutna bilar Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 
2. <span data-ttu-id="e9916-350">Aggressivt drivs fordon av modellen – vattenfallet diagram</span><span class="sxs-lookup"><span data-stu-id="e9916-350">Aggressively driven vehicles by model – Waterfall chart</span></span>  
   ![Vehicle telemetri - PIN-kod diagram 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

<span data-ttu-id="e9916-352">**Från sidan 5**, fästa följande:</span><span class="sxs-lookup"><span data-stu-id="e9916-352">**From Page 5**, pin the following:</span></span> 

1. <span data-ttu-id="e9916-353">Antal vin</span><span class="sxs-lookup"><span data-stu-id="e9916-353">Count of vin</span></span>    
   ![Vehicle telemetri - PIN-kod diagram 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2. <span data-ttu-id="e9916-355">Bränsle effektivt fordon av modell: grupperat stående stapeldiagram</span><span class="sxs-lookup"><span data-stu-id="e9916-355">Fuel efficient vehicles by model: Clustered column chart</span></span>  
   ![Vehicle telemetri - PIN-kod diagram 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

<span data-ttu-id="e9916-357">**Från sidan 4**, fästa följande:</span><span class="sxs-lookup"><span data-stu-id="e9916-357">**From Page 4**, pin the following:</span></span>  

1. <span data-ttu-id="e9916-358">Antal vin</span><span class="sxs-lookup"><span data-stu-id="e9916-358">Count of vin</span></span>  
   ![Vehicle telemetri - PIN-kod diagram 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 
2. <span data-ttu-id="e9916-360">Återkallade fordon efter ort, modell: Treemap</span><span class="sxs-lookup"><span data-stu-id="e9916-360">Recalled vehicles by city, model: Treemap</span></span>  
   ![Vehicle telemetri - PIN-kod diagram 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

<span data-ttu-id="e9916-362">**Från sidan 6**, fästa följande:</span><span class="sxs-lookup"><span data-stu-id="e9916-362">**From Page 6**, pin the following:</span></span>  

1. <span data-ttu-id="e9916-363">Contoso motorer-logotyp</span><span class="sxs-lookup"><span data-stu-id="e9916-363">Contoso Motors logo</span></span>  
   ![Vehicle telemetri - PIN-kod diagram 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

<span data-ttu-id="e9916-365">**Ordna instrumentpanelen**</span><span class="sxs-lookup"><span data-stu-id="e9916-365">**Organize the dashboard**</span></span>  

1. <span data-ttu-id="e9916-366">Gå till instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="e9916-366">Navigate to the dashboard</span></span>
2. <span data-ttu-id="e9916-367">Hovra över varje diagram och Byt namn på den baserad på namngivningen i fullständig instrumentpanelen bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="e9916-367">Hover over each chart and rename it based on the naming provided in the complete dashboard image below.</span></span> <span data-ttu-id="e9916-368">Även flytta diagrammen ska se ut som nedan instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9916-368">Also move the charts around to look like the dashboard below.</span></span>  
   ![Vehicle telemetri - ordna instrumentpanelen 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
   ![Vehicle telemetri Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3. <span data-ttu-id="e9916-371">Om du har skapat alla rapporter som nämns i detta dokument, sista slutförda instrumentpanelen bör se ut som följande bild.</span><span class="sxs-lookup"><span data-stu-id="e9916-371">If you have created all the reports as mentioned in this document, the final completed dashboard should look like the following figure.</span></span> 

![Vehicle telemetri - ordna instrumentpanelen 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

<span data-ttu-id="e9916-373">Grattis!</span><span class="sxs-lookup"><span data-stu-id="e9916-373">Congratulations!</span></span> <span data-ttu-id="e9916-374">Du har skapat rapporterna och instrumentpanelen för att få realtid, förutsägbara och batch-insikter om vehicle hälsa och köra vanor.</span><span class="sxs-lookup"><span data-stu-id="e9916-374">You have successfully created the reports and the dashboard to gain real-time, predictive and batch insights on vehicle health and driving habits.</span></span>  
