---
title: "aaaAdd en händelse källa tooyour Azure tid serien Insights miljö | Microsoft Docs"
description: "I kursen får ansluter du en händelse källa tooyour tid serien insikter miljö"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.openlocfilehash: 817df5e81cb4dc3d7376914a4651aabebadbcc32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-source-for-your-time-series-insights-environment-using-hello-ibiza-portal"></a><span data-ttu-id="4a0ba-103">Skapa en händelsekälla för tid serien insikter miljön med hjälp av hello Ibiza-portalen</span><span class="sxs-lookup"><span data-stu-id="4a0ba-103">Create an event source for your Time Series Insights environment using hello Ibiza portal</span></span>

<span data-ttu-id="4a0ba-104">Händelsekällan Time Series Insights härleds från en asynkron meddelandekö för händelser, som Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-104">Time Series Insights Event Source is derived from an event broker, like Azure Event Hubs.</span></span> <span data-ttu-id="4a0ba-105">Tid serien Insights ansluter direkt tooEvent källor, vill föra in hello dataströmmen utan användare toowrite en enda rad kod.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-105">Time Series Insights connects directly tooEvent Sources, ingesting hello data stream without requiring users toowrite a single line of code.</span></span> <span data-ttu-id="4a0ba-106">Tidserieinsikter stöder för närvarande, Azure Event Hubs och Azure IoT Hubs.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-106">Currently, Time Series Insights supports Azure Event Hubs and Azure IoT Hubs.</span></span> <span data-ttu-id="4a0ba-107">I framtida hello läggs mer händelsekällor till.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-107">In hello future, more Event Sources will be added.</span></span>

## <a name="steps-tooadd-an-event-source-tooyour-environment"></a><span data-ttu-id="4a0ba-108">Steg tooadd en händelse tooyour källmiljön</span><span class="sxs-lookup"><span data-stu-id="4a0ba-108">Steps tooadd an event source tooyour environment</span></span>

1.  <span data-ttu-id="4a0ba-109">Logga in toohello [Ibiza-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4a0ba-109">Sign in toohello [Ibiza portal](https://portal.azure.com).</span></span>
2.  <span data-ttu-id="4a0ba-110">Klicka på ”alla resurser” hello menyn hello vänster på hello Ibiza-portalen.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-110">Click “All resources” in hello menu on hello left side of hello Ibiza portal.</span></span>
3.  <span data-ttu-id="4a0ba-111">Välj Time Series Insights-miljö.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-111">Select your Time Series Insights environment.</span></span>

  ![Skapa hello tid serien insikter händelsekälla](media/add-event-source/getstarted-create-event-source-1.png)

4.  <span data-ttu-id="4a0ba-113">Välj ”händelsekällor” och klicka på ”+ Lägg till”.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-113">Select “Event Sources”, click “+ Add.”</span></span>

  ![Skapa hello tid serien insikter händelsekälla - information](media/add-event-source/getstarted-create-event-source-2.png)

5.  <span data-ttu-id="4a0ba-115">Ange hello namn hello händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-115">Specify hello name of hello event source.</span></span> <span data-ttu-id="4a0ba-116">Det här namnet är associerat med alla händelser som kommer från den här händelsekällan och är tillgänglig när en fråga körs.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-116">This name is associated with all events coming from this event source and is available at query time.</span></span>
6.  <span data-ttu-id="4a0ba-117">Välj en händelsehubb hello listan över Event Hub resurser i hello nuvarande prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-117">Select an event hub from hello list of Event Hub resources in hello current subscription.</span></span> <span data-ttu-id="4a0ba-118">Annars väljer du alternativ ”ange Event Hub-inställningar manuellt” toospecify en händelsehubb i en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-118">Otherwise choose import option "Provide Event Hub settings manually” toospecify an event hub in another subscription.</span></span> <span data-ttu-id="4a0ba-119">Händelser måste publiceras i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-119">Events must be published in JSON format.</span></span>
7.  <span data-ttu-id="4a0ba-120">Välj princip som har läsbehörighet i hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-120">Select policy that has read permission in hello event hub.</span></span>
8.  <span data-ttu-id="4a0ba-121">Välja konsumentgrupp för Event Hub.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-121">Specify event hub consumer group.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="4a0ba-122">Kontrollera att den här konsumentgruppen inte används av andra tjänster (t.ex Stream Analytics-jobb eller en annan Time Series Insights-miljö).</span><span class="sxs-lookup"><span data-stu-id="4a0ba-122">Make sure this consumer group is not used by any other service (such as Stream Analytics job or another Time Series Insights environment).</span></span> <span data-ttu-id="4a0ba-123">Om konsumentgrupp används av andra tjänster, Läs åtgärden påverkas negativt för den här miljön och hello andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-123">If consumer group is used by other services, read operation is negatively affected for this environment and hello other services.</span></span> <span data-ttu-id="4a0ba-124">Om du använder ”$Default” som hello konsumentgrupp leda toopotential återanvändning av andra läsare.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-124">If you are using “$Default” as hello consumer group, it could lead toopotential reuse by other readers.</span></span>

9.  <span data-ttu-id="4a0ba-125">Klicka på ”Skapa”.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-125">Click “Create.”</span></span>

<span data-ttu-id="4a0ba-126">Efter generering av hello händelsekälla startar automatiskt tid serien insikter strömmande data i din miljö.</span><span class="sxs-lookup"><span data-stu-id="4a0ba-126">After creation of hello event source, Time Series Insights will automatically start streaming data into your environment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a0ba-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a0ba-127">Next steps</span></span>

* <span data-ttu-id="4a0ba-128">[Skicka händelser](time-series-insights-send-events.md) toohello händelsekälla</span><span class="sxs-lookup"><span data-stu-id="4a0ba-128">[Send events](time-series-insights-send-events.md) toohello event source</span></span>
* <span data-ttu-id="4a0ba-129">Visa din miljö i [Time Series Insights-portalen](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="4a0ba-129">View your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
