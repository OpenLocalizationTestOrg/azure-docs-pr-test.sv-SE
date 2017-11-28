---
title: "aaaPredict vehicle hälsotillstånd och andra vanor - Azure | Microsoft Docs"
description: "Använda hello funktionerna i Cortana Intelligence toogain i realtid och förutsägbara insikter på vehicle hälsotillstånd och andra vanor."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 54cc890ff39493bc040bb809721388349665720f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a><span data-ttu-id="b2435-103">Fordonstelemetrianalys, lösning, playbook</span><span class="sxs-lookup"><span data-stu-id="b2435-103">Vehicle telemetry analytics solution playbook</span></span>
<span data-ttu-id="b2435-104">Detta **menyn** länkar toohello kapitlen i den här playbook.</span><span class="sxs-lookup"><span data-stu-id="b2435-104">This **menu** links toohello chapters in this playbook.</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a><span data-ttu-id="b2435-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="b2435-105">Overview</span></span>
<span data-ttu-id="b2435-106">Super datorer har flyttats från hello testmiljön och nu parkerade i vår garage!</span><span class="sxs-lookup"><span data-stu-id="b2435-106">Super computers have moved out of hello lab and are now parked in our garage!</span></span> <span data-ttu-id="b2435-107">Dessa senaste bilar innehåller en mängd sensorer, ger dem hello möjlighet tootrack och övervaka miljontals händelser per sekund.</span><span class="sxs-lookup"><span data-stu-id="b2435-107">These cutting-edge automobiles contain a myriad of sensors, giving them hello ability tootrack and monitor millions of events every second.</span></span> <span data-ttu-id="b2435-108">Vi räknar med att 2020, de flesta av dessa bilar kommer har anslutna toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="b2435-108">We expect that by 2020, most of these cars will have been connected toohello Internet.</span></span> <span data-ttu-id="b2435-109">Anta att trycka på i denna mängd data tooprovide större säkerhet, tillförlitlighet och en bättre intresseväckande upplevelse!</span><span class="sxs-lookup"><span data-stu-id="b2435-109">Imagine tapping into this wealth of data tooprovide greater safety, reliability and a better driving experience!</span></span> <span data-ttu-id="b2435-110">Microsoft har gjort detta drömma en verkligheten med Cortana Intelligence.</span><span class="sxs-lookup"><span data-stu-id="b2435-110">Microsoft has made this dream a reality with Cortana Intelligence.</span></span>

<span data-ttu-id="b2435-111">Microsofts Cortana Intelligence advanced analytics suite som gör att du tootransform dina data i intelligent åtgärd är en helt hanterad stordata.</span><span class="sxs-lookup"><span data-stu-id="b2435-111">Microsoft’s Cortana Intelligence is a fully managed big data and advanced analytics suite that enables you tootransform your data into intelligent action.</span></span> <span data-ttu-id="b2435-112">Vi vill toointroduce toohello Cortana Intelligence Vehicle telemetri Analytics Lösningsmall.</span><span class="sxs-lookup"><span data-stu-id="b2435-112">We want toointroduce you toohello Cortana Intelligence Vehicle Telemetry Analytics Solution Template.</span></span> <span data-ttu-id="b2435-113">Den här lösningen visar hur bil hos återförsäljarna, bil tillverkare och försäkringsbolag kan använda hello funktionerna i Cortana Intelligence toogain realtid och förutsägbara insikter om vehicle hälsa och köra vanor.</span><span class="sxs-lookup"><span data-stu-id="b2435-113">This solution demonstrates how car dealerships, automobile manufacturers, and insurance companies can use hello capabilities of Cortana Intelligence toogain real-time and predictive insights on vehicle health and driving habits.</span></span> 

<span data-ttu-id="b2435-114">hello lösningen har implementerats som en [lambda-arkitektur mönster](https://en.wikipedia.org/wiki/Lambda_architecture) visar hello fullt ut i hello Cortana Intelligence-plattformen för realtid och batchbearbetning.</span><span class="sxs-lookup"><span data-stu-id="b2435-114">hello solution is implemented as a [lambda architecture pattern](https://en.wikipedia.org/wiki/Lambda_architecture) showing hello full potential of hello Cortana Intelligence platform for real-time and batch processing.</span></span> <span data-ttu-id="b2435-115">hello-lösningen:</span><span class="sxs-lookup"><span data-stu-id="b2435-115">hello solution:</span></span> 

* <span data-ttu-id="b2435-116">ger en Vehicle telematik simulator</span><span class="sxs-lookup"><span data-stu-id="b2435-116">provides a Vehicle Telematics simulator</span></span>
* <span data-ttu-id="b2435-117">utnyttjar Händelsehubbar för att föra in miljontals simulerade vehicle telemetriska händelser till Azure</span><span class="sxs-lookup"><span data-stu-id="b2435-117">leverages Event Hubs for ingesting millions of simulated vehicle telemetry events into Azure</span></span> 
* <span data-ttu-id="b2435-118">använder Stream Analytics toogain realtidsinsikter på vehicle hälsa</span><span class="sxs-lookup"><span data-stu-id="b2435-118">uses Stream Analytics toogain real-time insights on vehicle health</span></span>
* <span data-ttu-id="b2435-119">håller kvar hello data till långsiktig lagring för bättre batch analytics.</span><span class="sxs-lookup"><span data-stu-id="b2435-119">persists hello data into long-term storage for richer batch analytics.</span></span> 
* <span data-ttu-id="b2435-120">drar nytta av Machine Learning för identifiering av avvikelse i realtid och batch-bearbetning toogain förutsägande insikter.</span><span class="sxs-lookup"><span data-stu-id="b2435-120">takes advantage of Machine Learning for anomaly detection in real-time and batch processing toogain predictive insights.</span></span>
* <span data-ttu-id="b2435-121">utnyttjar HDInsight tootransform data på skala och Data Factory toohandle orchestration, schemaläggning, resurshantering och övervakning av hello batch process-pipelinen</span><span class="sxs-lookup"><span data-stu-id="b2435-121">leverages HDInsight tootransform data at scale and Data Factory toohandle orchestration, scheduling, resource management, and monitoring of hello batch processing pipeline</span></span> 
* <span data-ttu-id="b2435-122">ger den här lösningen en omfattande instrumentpanel för data i realtid och förutsägelseanalys visualiseringar med Power BI</span><span class="sxs-lookup"><span data-stu-id="b2435-122">gives this solution a rich dashboard for real-time data and predictive analytics visualizations using Power BI</span></span>

## <a name="architecture"></a><span data-ttu-id="b2435-123">Arkitektur</span><span class="sxs-lookup"><span data-stu-id="b2435-123">Architecture</span></span>
<span data-ttu-id="b2435-124">![Arkitekturdiagram för lösningen](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*bild 1 – Vehicle telemetri Analytics lösningsarkitektur*</span><span class="sxs-lookup"><span data-stu-id="b2435-124">![Solution architecture diagram](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Figure 1 – Vehicle Telemetry Analytics Solution Architecture*</span></span>

<span data-ttu-id="b2435-125">Den här lösningen omfattar hello följande **Cortana Intelligence-komponenterna** och visar deras slutet tooend integrering:</span><span class="sxs-lookup"><span data-stu-id="b2435-125">This solution includes hello following **Cortana Intelligence components** and showcases their end tooend integration:</span></span>

* <span data-ttu-id="b2435-126">**Händelsehubbar** för att föra in miljontals vehicle telemetriska händelser i Azure.</span><span class="sxs-lookup"><span data-stu-id="b2435-126">**Event Hubs** for ingesting millions of vehicle telemetry events into Azure.</span></span>
* <span data-ttu-id="b2435-127">**Strömma Analytics** för att få realtidsinsikter på vehicle hälsa och kvarstår dessa data till långsiktig lagring för bättre batch analytics.</span><span class="sxs-lookup"><span data-stu-id="b2435-127">**Stream Analytics** for gaining real-time insights on vehicle health and persists that data into long-term storage for richer batch analytics.</span></span>
* <span data-ttu-id="b2435-128">**Machine Learning** för identifiering av avvikelse i realtid och batchbearbetning toogain förutsägande insikter.</span><span class="sxs-lookup"><span data-stu-id="b2435-128">**Machine Learning** for anomaly detection in real-time and batch processing toogain predictive insights.</span></span>
* <span data-ttu-id="b2435-129">**HDInsight** är balanserad tootransform data i skala</span><span class="sxs-lookup"><span data-stu-id="b2435-129">**HDInsight** is leveraged tootransform data at scale</span></span>
* <span data-ttu-id="b2435-130">**Data Factory** hanterar orchestration, schemaläggning, resurshantering och övervakning av hello batch process-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="b2435-130">**Data Factory** handles orchestration, scheduling, resource management and monitoring of hello batch processing pipeline.</span></span>
* <span data-ttu-id="b2435-131">**Power BI** ger den här lösningen en omfattande instrumentpanel för data i realtid och förutsägelseanalys visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="b2435-131">**Power BI** gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span>

<span data-ttu-id="b2435-132">Den här lösningen använder två olika **datakällor**:</span><span class="sxs-lookup"><span data-stu-id="b2435-132">This solution accesses two different **data sources**:</span></span> 

* <span data-ttu-id="b2435-133">**Simulerade vehicle signaler och diagnostik**: vehicle telematik simulator avger diagnostisk information och signalerar som motsvarar toohello tillståndet för hello vehicle och hello körning mönster vid en viss tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="b2435-133">**Simulated vehicle signals and diagnostics**: A vehicle telematics simulator emits diagnostic information and signals that correspond toohello state of hello vehicle and hello driving pattern at a given point in time.</span></span> 
* <span data-ttu-id="b2435-134">**Vehicle katalogen**: en referens dataset som innehåller en VIN toomodel mappning.</span><span class="sxs-lookup"><span data-stu-id="b2435-134">**Vehicle catalog**: A reference dataset containing a VIN toomodel mapping.</span></span>

