---
title: "aaaAdd referens datauppsättning tooyour Azure tid serien Insights miljö | Microsoft Docs"
description: "Lägg till referens datauppsättning tooyour tid serien insikter miljö i den här självstudiekursen"
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
ms.openlocfilehash: 05e626ed81a22f2a8710b23a931ccd17c0f38ca5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-hello-ibiza-portal"></a><span data-ttu-id="679d9-103">Skapa en referens för datauppsättning för tid serien insikter miljön med hjälp av hello Ibiza-portalen</span><span class="sxs-lookup"><span data-stu-id="679d9-103">Create a reference data set for your Time Series Insights environment using hello Ibiza portal</span></span>

<span data-ttu-id="679d9-104">En referens datauppsättning är en samling objekt som är förstärkta med hello händelser från din händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="679d9-104">A Reference Data Set is a collection of items that are augmented with hello events from your event source.</span></span> <span data-ttu-id="679d9-105">Bearbetningsmotorn för tidsserieinsikter kopplar en händelse från din händelsekälla till ett objekt i referensdatauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="679d9-105">Time Series Insights ingress engine joins an event from your event source with an item in your reference data set.</span></span> <span data-ttu-id="679d9-106">Den här förhöjda händelsen är sedan tillgängliga för frågor.</span><span class="sxs-lookup"><span data-stu-id="679d9-106">This augmented event is then available for query.</span></span> <span data-ttu-id="679d9-107">Den här kopplingen baseras på hello nycklar som definierats i datauppsättningen referens.</span><span class="sxs-lookup"><span data-stu-id="679d9-107">This join is based on hello keys defined in your reference data set.</span></span>

## <a name="steps-tooadd-a-reference-data-set-tooyour-environment"></a><span data-ttu-id="679d9-108">Steg tooadd en referens datauppsättning tooyour miljö</span><span class="sxs-lookup"><span data-stu-id="679d9-108">Steps tooadd a reference data set tooyour environment</span></span>

1. <span data-ttu-id="679d9-109">Logga in toohello [Ibiza-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="679d9-109">Sign in toohello [Ibiza portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="679d9-110">Klicka på ”alla resurser” hello menyn hello vänster på hello Ibiza-portalen.</span><span class="sxs-lookup"><span data-stu-id="679d9-110">Click “All resources” in hello menu on hello left side of hello Ibiza portal.</span></span>
3. <span data-ttu-id="679d9-111">Välj Time Series Insights-miljö.</span><span class="sxs-lookup"><span data-stu-id="679d9-111">Select your Time Series Insights environment.</span></span>

    ![Skapa hello tid serien insikter referens datauppsättning](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. <span data-ttu-id="679d9-113">Välj ”referensdatauppsättningar”, klicka på ”+ Lägg till”.</span><span class="sxs-lookup"><span data-stu-id="679d9-113">Select “Reference Data Sets”, click “+ Add.”</span></span>

    ![Skapa datauppsättning för hello tid serien insikter referens - information](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

5. <span data-ttu-id="679d9-115">Ange hello namn hello referens datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="679d9-115">Specify hello name of hello reference data set.</span></span>
6. <span data-ttu-id="679d9-116">Ange hello nyckelnamn och dess.</span><span class="sxs-lookup"><span data-stu-id="679d9-116">Specify hello key name and its type.</span></span> <span data-ttu-id="679d9-117">Den här namn och typ är används toopick hello rätt egenskap från hello händelse i din händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="679d9-117">This name and type is used toopick hello correct property from hello event in your event source.</span></span> <span data-ttu-id="679d9-118">Till exempel om du tillhandahåller nyckelnamn som ”DeviceId” och typ som ”sträng” sedan hello tid serien insikter ingång letar efter en egenskap med namnet hello ”DeviceId” av typen ”sträng” i hello inkommande händelse.</span><span class="sxs-lookup"><span data-stu-id="679d9-118">For instance, if you provide key name as “DeviceId” and type as “String”, then hello Time Series Insights ingress engine looks for a property with hello name “DeviceId” of type “String” in hello incoming event.</span></span> <span data-ttu-id="679d9-119">Du kan ange flera viktiga toojoin med hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="679d9-119">You can provide more than one key toojoin with hello event.</span></span> <span data-ttu-id="679d9-120">hello egenskapen matchar är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="679d9-120">hello property name match is case-sensitive.</span></span>

     ![Skapa datauppsättning för hello tid serien insikter referens - information](media/add-reference-data-set/getstarted-create-reference-data-set-3.png)

7. <span data-ttu-id="679d9-122">Klicka på ”Skapa”.</span><span class="sxs-lookup"><span data-stu-id="679d9-122">Click “Create.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="679d9-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="679d9-123">Next steps</span></span>

* <span data-ttu-id="679d9-124">[Hantera referensdata](time-series-insights-manage-reference-data-csharp.md) programmässigt.</span><span class="sxs-lookup"><span data-stu-id="679d9-124">[Manage reference data](time-series-insights-manage-reference-data-csharp.md) programmatically.</span></span>
* <span data-ttu-id="679d9-125">Hello fullständiga API-referens, se [referens Data API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="679d9-125">For hello complete API reference, see [Reference Data API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) document.</span></span>
