---
title: "Skapa en Azure Time Series Insights-miljö | Microsoft Docs"
description: "I den här självstudien får du lära dig att skapa en Time Series-miljö, ansluta den till en händelsekälla och redo att analysera dina händelsedata på några minuter."
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
ms.openlocfilehash: eb710795916a2d7beea75a6408a0982fb4dc8750
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a><span data-ttu-id="74d87-103">Skapa en ny Time Series Insights-miljö i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="74d87-103">Create a new Time Series Insights environment in the Azure portal</span></span>

<span data-ttu-id="74d87-104">Time Series Insights-miljön är en Azure-resurs med ingångs- och lagringskapacitet.</span><span class="sxs-lookup"><span data-stu-id="74d87-104">Time Series Insights environment is an Azure resource with ingress and storage capacity.</span></span> <span data-ttu-id="74d87-105">Kunder etablerar miljöer via Azure Portal med den kapacitet som krävs.</span><span class="sxs-lookup"><span data-stu-id="74d87-105">Customers provision environments via the Azure portal with the required capacity.</span></span>

## <a name="steps-to-create-the-environment"></a><span data-ttu-id="74d87-106">Anvisningar för att skapa miljön</span><span class="sxs-lookup"><span data-stu-id="74d87-106">Steps to create the environment</span></span>

<span data-ttu-id="74d87-107">Följ de här anvisningarna för att skapa en miljö:</span><span class="sxs-lookup"><span data-stu-id="74d87-107">Follow these steps to create your environment:</span></span>

1.  <span data-ttu-id="74d87-108">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="74d87-108">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2.  <span data-ttu-id="74d87-109">Klicka på plustecknet (”+”) i det övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="74d87-109">Click the plus sign (“+”) in the top left corner.</span></span>
3.  <span data-ttu-id="74d87-110">Sök efter ”Time Series Insights” i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="74d87-110">Search for “Time Series Insights” in the search box.</span></span>

  ![Skapa Time Series Insights-miljön](media/get-started/getstarted-create-environment1.png)

4.  <span data-ttu-id="74d87-112">Välj ”Time Series Insights”, klicka på ”Skapa”.</span><span class="sxs-lookup"><span data-stu-id="74d87-112">Select “Time Series Insights”, click “Create”.</span></span>

  ![Skapa Time Series Insights-resursgruppen](media/get-started/getstarted-create-environment2.png)

5.  <span data-ttu-id="74d87-114">Ange ett namn på miljön.</span><span class="sxs-lookup"><span data-stu-id="74d87-114">Specify environment name.</span></span> <span data-ttu-id="74d87-115">Namnet representerar miljön i [time series explorer](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="74d87-115">This name will represent the environment in [time series explorer](https://insights.timeseries.azure.com).</span></span>
6.  <span data-ttu-id="74d87-116">Välj en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="74d87-116">Select a subscription.</span></span> <span data-ttu-id="74d87-117">Välj ett namn som innehåller din händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="74d87-117">Choose one that contains your event source.</span></span> <span data-ttu-id="74d87-118">Time Series Insights kan automatiskt identifiera Azure IoT Hub- och Event Hub-resurser som existerar i samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="74d87-118">Time Series Insights can auto-detect Azure IoT Hub and Event Hub resources existing in the same subscription.</span></span>
7.  <span data-ttu-id="74d87-119">Välj eller skapa en Resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="74d87-119">Select or create a resource group.</span></span> <span data-ttu-id="74d87-120">En resursgrupp är en samling med Azure-resurser som används tillsammans.</span><span class="sxs-lookup"><span data-stu-id="74d87-120">A resource group is a collection of Azure resources used together.</span></span>
8.  <span data-ttu-id="74d87-121">Välj en värdplats.</span><span class="sxs-lookup"><span data-stu-id="74d87-121">Select a hosting location.</span></span> <span data-ttu-id="74d87-122">Välj en plats som innehåller din källa för att undvika att flytta data mellan datacenter.</span><span class="sxs-lookup"><span data-stu-id="74d87-122">To avoid moving data across data centers, choose location that contains your event source.</span></span>
9.  <span data-ttu-id="74d87-123">Välj en prisnivå.</span><span class="sxs-lookup"><span data-stu-id="74d87-123">Select a pricing tier.</span></span>
10. <span data-ttu-id="74d87-124">Välj kapacitet.</span><span class="sxs-lookup"><span data-stu-id="74d87-124">Select capacity.</span></span> <span data-ttu-id="74d87-125">Du kan ändra kapacitet för en miljö när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="74d87-125">You can change capacity of an environment after creation.</span></span>
11. <span data-ttu-id="74d87-126">Skapa din miljö.</span><span class="sxs-lookup"><span data-stu-id="74d87-126">Create your environment.</span></span> <span data-ttu-id="74d87-127">Du kan också fästa miljön på instrumentpanelen för enkel åtkomst när du loggar in.</span><span class="sxs-lookup"><span data-stu-id="74d87-127">You can also pin your environment to the dashboard for easy access whenever you sign in.</span></span>

  ![Fäst Time Series Insights vid instrumentpanelen](media/get-started/getstarted-create-environment3.png)

## <a name="next-steps"></a><span data-ttu-id="74d87-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="74d87-129">Next steps</span></span>

* <span data-ttu-id="74d87-130">[Definiera dataåtkomstprinciper](time-series-insights-data-access.md) för att få åtkomst till miljön i [Time Series Insights-portalen](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="74d87-130">[Define data access policies](time-series-insights-data-access.md) to access your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
* [<span data-ttu-id="74d87-131">Skapa en händelsekälla</span><span class="sxs-lookup"><span data-stu-id="74d87-131">Create an event source</span></span>](time-series-insights-add-event-source.md)
* <span data-ttu-id="74d87-132">[Skicka händelser](time-series-insights-send-events.md) till händelsekällan</span><span class="sxs-lookup"><span data-stu-id="74d87-132">[Send events](time-series-insights-send-events.md) to the event source</span></span>
