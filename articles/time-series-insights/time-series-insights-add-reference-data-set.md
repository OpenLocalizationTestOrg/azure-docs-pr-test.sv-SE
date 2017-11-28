---
title: "Lägga till en referensdatauppsättning i miljön för Azure Time Series Insights | Microsoft Docs"
description: "I den här självstudien ansluter du en referensdatauppsättning till Time Series Insights-miljön"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: venkatja
ms.openlocfilehash: 23444297b5231e6a026bcd9ce3ee9f943bf05867
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-ibiza-portal"></a><span data-ttu-id="15341-103">Skapa en referensdatauppsättning för miljön för Time Series Insights med Ibiza Portal</span><span class="sxs-lookup"><span data-stu-id="15341-103">Create a reference data set for your Time Series Insights environment using the Ibiza portal</span></span>

<span data-ttu-id="15341-104">En referensdatauppsättning är en samling objekt som är förstärkta med händelser från din händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="15341-104">A Reference Data Set is a collection of items that are augmented with the events from your event source.</span></span> <span data-ttu-id="15341-105">Bearbetningsmotorn för tidsserieinsikter kopplar en händelse från din händelsekälla till ett objekt i referensdatauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="15341-105">Time Series Insights ingress engine joins an event from your event source with an item in your reference data set.</span></span> <span data-ttu-id="15341-106">Den här förhöjda händelsen är sedan tillgängliga för frågor.</span><span class="sxs-lookup"><span data-stu-id="15341-106">This augmented event is then available for query.</span></span> <span data-ttu-id="15341-107">Den här kopplingen baseras på de nycklar som definierats i referensdatauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="15341-107">This join is based on the keys defined in your reference data set.</span></span>

## <a name="steps-to-add-a-reference-data-set-to-your-environment"></a><span data-ttu-id="15341-108">Steg för att lägga till en referensdatauppsättning i din miljö</span><span class="sxs-lookup"><span data-stu-id="15341-108">Steps to add a reference data set to your environment</span></span>

1. <span data-ttu-id="15341-109">Logga in på [Ibiza portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="15341-109">Sign in to the [Ibiza portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="15341-110">Klicka på ”Alla resurser” på menyn på vänster sida av Ibiza Portal.</span><span class="sxs-lookup"><span data-stu-id="15341-110">Click “All resources” in the menu on the left side of the Ibiza portal.</span></span>
3. <span data-ttu-id="15341-111">Välj Time Series Insights-miljö.</span><span class="sxs-lookup"><span data-stu-id="15341-111">Select your Time Series Insights environment.</span></span>

    ![Skapa Time Series Insights-referensdatauppsättningen](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. <span data-ttu-id="15341-113">Välj ”referensdatauppsättningar”, klicka på ”+ Lägg till”.</span><span class="sxs-lookup"><span data-stu-id="15341-113">Select “Reference Data Sets”, click “+ Add.”</span></span>

    ![Skapa Time Series Insights-referensdatauppsättningen - information](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

5. <span data-ttu-id="15341-115">Ange namnet på referensdatauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="15341-115">Specify the name of the reference data set.</span></span>
6. <span data-ttu-id="15341-116">Ange namnet och dess typ.</span><span class="sxs-lookup"><span data-stu-id="15341-116">Specify the key name and its type.</span></span> <span data-ttu-id="15341-117">Detta namn och typ används för att välja rätt egenskap från händelsen i din händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="15341-117">This name and type is used to pick the correct property from the event in your event source.</span></span> <span data-ttu-id="15341-118">Till exempel, om du tillhandahåller nyckelnamn som ”DeviceId” och typ som ”String” söker motorn för tidserieinsikter sedan en egenskap med namnet ”DeviceId” av typen ”String” i den inkommande händelsen.</span><span class="sxs-lookup"><span data-stu-id="15341-118">For instance, if you provide key name as “DeviceId” and type as “String”, then the Time Series Insights ingress engine looks for a property with the name “DeviceId” of type “String” in the incoming event.</span></span> <span data-ttu-id="15341-119">Du kan ange mer än en nyckel att ansluta till händelsen.</span><span class="sxs-lookup"><span data-stu-id="15341-119">You can provide more than one key to join with the event.</span></span> <span data-ttu-id="15341-120">Matchningen med egenskapsnamn är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="15341-120">The property name match is case-sensitive.</span></span>

     ![Skapa Time Series Insights-referensdatauppsättningen - information](media/add-reference-data-set/getstarted-create-reference-data-set-3.png)

7. <span data-ttu-id="15341-122">Klicka på ”Skapa”.</span><span class="sxs-lookup"><span data-stu-id="15341-122">Click “Create.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="15341-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="15341-123">Next steps</span></span>

* <span data-ttu-id="15341-124">[Hantera referensdata](time-series-insights-manage-reference-data-csharp.md) programmässigt.</span><span class="sxs-lookup"><span data-stu-id="15341-124">[Manage reference data](time-series-insights-manage-reference-data-csharp.md) programmatically.</span></span>
* <span data-ttu-id="15341-125">En fullständig API-referens, se dokumentet [Referensdata-API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) .</span><span class="sxs-lookup"><span data-stu-id="15341-125">For the complete API reference, see [Reference Data API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) document.</span></span>