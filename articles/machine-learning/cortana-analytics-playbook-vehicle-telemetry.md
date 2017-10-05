---
title: "Förutsäga vehicle hälsotillstånd och andra vanor - Azure | Microsoft Docs"
description: "Använda funktionerna i Cortana Intelligence och få insikter om i realtid och förutsägbara på vehicle hälsa och köra vanor."
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
ms.openlocfilehash: d202d314c61416cf306f760f93e0a4a88a1ab42b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a><span data-ttu-id="94252-103">Fordonstelemetrianalys, lösning, playbook</span><span class="sxs-lookup"><span data-stu-id="94252-103">Vehicle telemetry analytics solution playbook</span></span>
<span data-ttu-id="94252-104">Detta **menyn** länkar till kapitlen i den här playbook.</span><span class="sxs-lookup"><span data-stu-id="94252-104">This **menu** links to the chapters in this playbook.</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a><span data-ttu-id="94252-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="94252-105">Overview</span></span>
<span data-ttu-id="94252-106">Super datorer har flyttats från labbet och nu parkerade i vår garage!</span><span class="sxs-lookup"><span data-stu-id="94252-106">Super computers have moved out of the lab and are now parked in our garage!</span></span> <span data-ttu-id="94252-107">Dessa senaste bilar innehåller en mängd sensorer, ger dem möjlighet att spåra och övervaka miljontals händelser per sekund.</span><span class="sxs-lookup"><span data-stu-id="94252-107">These cutting-edge automobiles contain a myriad of sensors, giving them the ability to track and monitor millions of events every second.</span></span> <span data-ttu-id="94252-108">Vi räknar med att 2020, de flesta av dessa bilar kommer har anslutits till Internet.</span><span class="sxs-lookup"><span data-stu-id="94252-108">We expect that by 2020, most of these cars will have been connected to the Internet.</span></span> <span data-ttu-id="94252-109">Anta att trycka på i denna mängd data för att ge större säkerhet, tillförlitlighet och en bättre intresseväckande upplevelse!</span><span class="sxs-lookup"><span data-stu-id="94252-109">Imagine tapping into this wealth of data to provide greater safety, reliability and a better driving experience!</span></span> <span data-ttu-id="94252-110">Microsoft har gjort detta drömma en verkligheten med Cortana Intelligence.</span><span class="sxs-lookup"><span data-stu-id="94252-110">Microsoft has made this dream a reality with Cortana Intelligence.</span></span>

<span data-ttu-id="94252-111">Microsofts Cortana Intelligence är en helt hanterad stordata och avancerade analyser suite som gör att du kan omvandla dina data till intelligent åtgärd.</span><span class="sxs-lookup"><span data-stu-id="94252-111">Microsoft’s Cortana Intelligence is a fully managed big data and advanced analytics suite that enables you to transform your data into intelligent action.</span></span> <span data-ttu-id="94252-112">Vi vill introduktion till Lösningsmall Cortana Intelligence Vehicle telemetri Analytics.</span><span class="sxs-lookup"><span data-stu-id="94252-112">We want to introduce you to the Cortana Intelligence Vehicle Telemetry Analytics Solution Template.</span></span> <span data-ttu-id="94252-113">Den här lösningen visar hur bil hos återförsäljarna, bil tillverkare och försäkringsbolag kan använda funktionerna i Cortana Intelligence för att få realtid och förutsägbara insikter om vehicle hälsa och köra vanor.</span><span class="sxs-lookup"><span data-stu-id="94252-113">This solution demonstrates how car dealerships, automobile manufacturers, and insurance companies can use the capabilities of Cortana Intelligence to gain real-time and predictive insights on vehicle health and driving habits.</span></span> 

<span data-ttu-id="94252-114">Lösningen har implementerats som en [lambda-arkitektur mönster](https://en.wikipedia.org/wiki/Lambda_architecture) visar hela potentiella i Cortana Intelligence-plattformen för realtid och batchbearbetning.</span><span class="sxs-lookup"><span data-stu-id="94252-114">The solution is implemented as a [lambda architecture pattern](https://en.wikipedia.org/wiki/Lambda_architecture) showing the full potential of the Cortana Intelligence platform for real-time and batch processing.</span></span> <span data-ttu-id="94252-115">Lösning:</span><span class="sxs-lookup"><span data-stu-id="94252-115">The solution:</span></span> 

* <span data-ttu-id="94252-116">ger en Vehicle telematik simulator</span><span class="sxs-lookup"><span data-stu-id="94252-116">provides a Vehicle Telematics simulator</span></span>
* <span data-ttu-id="94252-117">utnyttjar Händelsehubbar för att föra in miljontals simulerade vehicle telemetriska händelser till Azure</span><span class="sxs-lookup"><span data-stu-id="94252-117">leverages Event Hubs for ingesting millions of simulated vehicle telemetry events into Azure</span></span> 
* <span data-ttu-id="94252-118">använder Stream Analytics för att få realtidsinsikter på vehicle hälsa</span><span class="sxs-lookup"><span data-stu-id="94252-118">uses Stream Analytics to gain real-time insights on vehicle health</span></span>
* <span data-ttu-id="94252-119">sparar data till långsiktig lagring för bättre batch analytics.</span><span class="sxs-lookup"><span data-stu-id="94252-119">persists the data into long-term storage for richer batch analytics.</span></span> 
* <span data-ttu-id="94252-120">drar nytta av Machine Learning för identifiering av avvikelse i realtid och batch-bearbetning för att få förutsägande insikter.</span><span class="sxs-lookup"><span data-stu-id="94252-120">takes advantage of Machine Learning for anomaly detection in real-time and batch processing to gain predictive insights.</span></span>
* <span data-ttu-id="94252-121">utnyttjar HDInsight för att omvandla data i större skala och Data Factory för att hantera orchestration, schemaläggning, resurshantering och övervakning av batch-bearbetning pipeline</span><span class="sxs-lookup"><span data-stu-id="94252-121">leverages HDInsight to transform data at scale and Data Factory to handle orchestration, scheduling, resource management, and monitoring of the batch processing pipeline</span></span> 
* <span data-ttu-id="94252-122">ger den här lösningen en omfattande instrumentpanel för data i realtid och förutsägelseanalys visualiseringar med Power BI</span><span class="sxs-lookup"><span data-stu-id="94252-122">gives this solution a rich dashboard for real-time data and predictive analytics visualizations using Power BI</span></span>

## <a name="architecture"></a><span data-ttu-id="94252-123">Arkitektur</span><span class="sxs-lookup"><span data-stu-id="94252-123">Architecture</span></span>
<span data-ttu-id="94252-124">![Arkitekturdiagram för lösningen](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*bild 1 – Vehicle telemetri Analytics lösningsarkitektur*</span><span class="sxs-lookup"><span data-stu-id="94252-124">![Solution architecture diagram](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Figure 1 – Vehicle Telemetry Analytics Solution Architecture*</span></span>

<span data-ttu-id="94252-125">Den här lösningen innehåller följande **Cortana Intelligence-komponenterna** och visar deras slutpunkt till slutpunkt-integrering:</span><span class="sxs-lookup"><span data-stu-id="94252-125">This solution includes the following **Cortana Intelligence components** and showcases their end to end integration:</span></span>

* <span data-ttu-id="94252-126">**Händelsehubbar** för att föra in miljontals vehicle telemetriska händelser i Azure.</span><span class="sxs-lookup"><span data-stu-id="94252-126">**Event Hubs** for ingesting millions of vehicle telemetry events into Azure.</span></span>
* <span data-ttu-id="94252-127">**Strömma Analytics** för att få realtidsinsikter på vehicle hälsa och kvarstår dessa data till långsiktig lagring för bättre batch analytics.</span><span class="sxs-lookup"><span data-stu-id="94252-127">**Stream Analytics** for gaining real-time insights on vehicle health and persists that data into long-term storage for richer batch analytics.</span></span>
* <span data-ttu-id="94252-128">**Machine Learning** för identifiering av avvikelse i realtid och batchbearbetning och få förutsägande insikter.</span><span class="sxs-lookup"><span data-stu-id="94252-128">**Machine Learning** for anomaly detection in real-time and batch processing to gain predictive insights.</span></span>
* <span data-ttu-id="94252-129">**HDInsight** utnyttjas för att omvandla data i skala</span><span class="sxs-lookup"><span data-stu-id="94252-129">**HDInsight** is leveraged to transform data at scale</span></span>
* <span data-ttu-id="94252-130">**Data Factory** hanterar orchestration, schemaläggning, resurshantering och övervakning av batch-bearbetning-pipeline.</span><span class="sxs-lookup"><span data-stu-id="94252-130">**Data Factory** handles orchestration, scheduling, resource management and monitoring of the batch processing pipeline.</span></span>
* <span data-ttu-id="94252-131">**Power BI** ger den här lösningen en omfattande instrumentpanel för data i realtid och förutsägelseanalys visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="94252-131">**Power BI** gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span>

<span data-ttu-id="94252-132">Den här lösningen använder två olika **datakällor**:</span><span class="sxs-lookup"><span data-stu-id="94252-132">This solution accesses two different **data sources**:</span></span> 

* <span data-ttu-id="94252-133">**Simulerade vehicle signaler och diagnostik**: vehicle telematik simulator avger diagnostisk information och signalerar som motsvarar tillstånd fordon och intresseväckande mönstret vid en viss tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="94252-133">**Simulated vehicle signals and diagnostics**: A vehicle telematics simulator emits diagnostic information and signals that correspond to the state of the vehicle and the driving pattern at a given point in time.</span></span> 
* <span data-ttu-id="94252-134">**Vehicle katalogen**: en referens datamängd som innehåller Registreringsnumret för Modellmappning.</span><span class="sxs-lookup"><span data-stu-id="94252-134">**Vehicle catalog**: A reference dataset containing a VIN to model mapping.</span></span>

