---
title: "aaaCreate en Azure tid serien Insights miljö | Microsoft Docs"
description: "I kursen får du lära dig hur toocreate serie miljön, ansluter du det tooan händelsekälla och klara tooanalyze din händelsedata i minuter."
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
ms.openlocfilehash: 7120fc9a6e4d4a4972f8cb37e4d9945cfb746fd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-new-time-series-insights-environment-in-hello-azure-portal"></a><span data-ttu-id="18df8-103">Skapa en ny tid serien insikter miljö i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="18df8-103">Create a new Time Series Insights environment in hello Azure portal</span></span>

<span data-ttu-id="18df8-104">Time Series Insights-miljön är en Azure-resurs med ingångs- och lagringskapacitet.</span><span class="sxs-lookup"><span data-stu-id="18df8-104">Time Series Insights environment is an Azure resource with ingress and storage capacity.</span></span> <span data-ttu-id="18df8-105">Kunder etablera miljöer via hello Azure-portalen med hello krävs kapacitet.</span><span class="sxs-lookup"><span data-stu-id="18df8-105">Customers provision environments via hello Azure portal with hello required capacity.</span></span>

## <a name="steps-toocreate-hello-environment"></a><span data-ttu-id="18df8-106">Steg toocreate hello miljö</span><span class="sxs-lookup"><span data-stu-id="18df8-106">Steps toocreate hello environment</span></span>

<span data-ttu-id="18df8-107">Följ dessa steg toocreate din miljö:</span><span class="sxs-lookup"><span data-stu-id="18df8-107">Follow these steps toocreate your environment:</span></span>

1.  <span data-ttu-id="18df8-108">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="18df8-108">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2.  <span data-ttu-id="18df8-109">Klicka hello plustecken (”+”) i hello övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="18df8-109">Click hello plus sign (“+”) in hello top left corner.</span></span>
3.  <span data-ttu-id="18df8-110">Sök efter ”tid serien insikter” i sökrutan för hello.</span><span class="sxs-lookup"><span data-stu-id="18df8-110">Search for “Time Series Insights” in hello search box.</span></span>

  ![Skapa hello tid serien insikter miljö](media/get-started/getstarted-create-environment1.png)

4.  <span data-ttu-id="18df8-112">Välj ”Time Series Insights”, klicka på ”Skapa”.</span><span class="sxs-lookup"><span data-stu-id="18df8-112">Select “Time Series Insights”, click “Create”.</span></span>

  ![Skapa hello tid serien insikter resursgrupp](media/get-started/getstarted-create-environment2.png)

5.  <span data-ttu-id="18df8-114">Ange ett namn på miljön.</span><span class="sxs-lookup"><span data-stu-id="18df8-114">Specify environment name.</span></span> <span data-ttu-id="18df8-115">Det här namnet representerar hello-miljö i [tid serien explorer](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="18df8-115">This name will represent hello environment in [time series explorer](https://insights.timeseries.azure.com).</span></span>
6.  <span data-ttu-id="18df8-116">Välj en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="18df8-116">Select a subscription.</span></span> <span data-ttu-id="18df8-117">Välj ett namn som innehåller din händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="18df8-117">Choose one that contains your event source.</span></span> <span data-ttu-id="18df8-118">Tid serien insikter kan automatisk identifiering av Azure IoT Hub och Event Hub-resurser finns i hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="18df8-118">Time Series Insights can auto-detect Azure IoT Hub and Event Hub resources existing in hello same subscription.</span></span>
7.  <span data-ttu-id="18df8-119">Välj eller skapa en Resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="18df8-119">Select or create a resource group.</span></span> <span data-ttu-id="18df8-120">En resursgrupp är en samling med Azure-resurser som används tillsammans.</span><span class="sxs-lookup"><span data-stu-id="18df8-120">A resource group is a collection of Azure resources used together.</span></span>
8.  <span data-ttu-id="18df8-121">Välj en värdplats.</span><span class="sxs-lookup"><span data-stu-id="18df8-121">Select a hosting location.</span></span> <span data-ttu-id="18df8-122">tooavoid flytta över data datacenter, välj platsen som innehåller din händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="18df8-122">tooavoid moving data across data centers, choose location that contains your event source.</span></span>
9.  <span data-ttu-id="18df8-123">Välj en prisnivå.</span><span class="sxs-lookup"><span data-stu-id="18df8-123">Select a pricing tier.</span></span>
10. <span data-ttu-id="18df8-124">Välj kapacitet.</span><span class="sxs-lookup"><span data-stu-id="18df8-124">Select capacity.</span></span> <span data-ttu-id="18df8-125">Du kan ändra kapacitet för en miljö när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="18df8-125">You can change capacity of an environment after creation.</span></span>
11. <span data-ttu-id="18df8-126">Skapa din miljö.</span><span class="sxs-lookup"><span data-stu-id="18df8-126">Create your environment.</span></span> <span data-ttu-id="18df8-127">Du kan även fästa miljö toohello instrumentpanelen för enkel åtkomst när du loggar in.</span><span class="sxs-lookup"><span data-stu-id="18df8-127">You can also pin your environment toohello dashboard for easy access whenever you sign in.</span></span>

  ![Skapa hello tid serien insikter PIN-kod toodashboard](media/get-started/getstarted-create-environment3.png)

## <a name="next-steps"></a><span data-ttu-id="18df8-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18df8-129">Next steps</span></span>

* <span data-ttu-id="18df8-130">[Definiera principer för åtkomst av data](time-series-insights-data-access.md) tooaccess miljön i [tid serien Insights-portalen](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="18df8-130">[Define data access policies](time-series-insights-data-access.md) tooaccess your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
* [<span data-ttu-id="18df8-131">Skapa en händelsekälla</span><span class="sxs-lookup"><span data-stu-id="18df8-131">Create an event source</span></span>](time-series-insights-add-event-source.md)
* <span data-ttu-id="18df8-132">[Skicka händelser](time-series-insights-send-events.md) toohello händelsekälla</span><span class="sxs-lookup"><span data-stu-id="18df8-132">[Send events](time-series-insights-send-events.md) toohello event source</span></span>
