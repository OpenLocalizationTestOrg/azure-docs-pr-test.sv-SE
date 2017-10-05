---
title: "Så här skalar du Azure tid serien Insights miljön | Microsoft Docs"
description: "Den här kursen ingår så här skalar du Azure tid serien Insights-miljö"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: 8f6c66ea2173c98179ec899d6626c2ab6f7ec4b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-scale-your-time-series-insights-environment"></a><span data-ttu-id="4daa7-103">Så här skalar du tid serien insikter miljön</span><span class="sxs-lookup"><span data-stu-id="4daa7-103">How to scale your Time Series Insights environment</span></span>

<span data-ttu-id="4daa7-104">Den här kursen ingår så här skalar du tid serien insikter miljön.</span><span class="sxs-lookup"><span data-stu-id="4daa7-104">This tutorial covers how to scale your Time Series Insights environment.</span></span>

> [!NOTE]
> <span data-ttu-id="4daa7-105">Skala upp över sku-typer tillåts inte.</span><span class="sxs-lookup"><span data-stu-id="4daa7-105">Scale up across sku types is not allowed.</span></span> <span data-ttu-id="4daa7-106">En miljö med en S1 Sku kan inte konverteras till en S2 miljö.</span><span class="sxs-lookup"><span data-stu-id="4daa7-106">An environment with a S1 Sku cannot be converted into an S2 environment.</span></span>

## <a name="s1-sku-ingress-rates-and-capacities"></a><span data-ttu-id="4daa7-107">S1-SKU ingång priser och kapacitet</span><span class="sxs-lookup"><span data-stu-id="4daa7-107">S1 SKU ingress rates and capacities</span></span>

| <span data-ttu-id="4daa7-108">S1 Artikelnummerkapaciteten</span><span class="sxs-lookup"><span data-stu-id="4daa7-108">S1 SKU Capacity</span></span> | <span data-ttu-id="4daa7-109">Ingång hastighet</span><span class="sxs-lookup"><span data-stu-id="4daa7-109">Ingress Rate</span></span> | <span data-ttu-id="4daa7-110">Maximal lagringskapacitet</span><span class="sxs-lookup"><span data-stu-id="4daa7-110">Maximum Storage Capacity</span></span>
| --- | --- | --- |
| <span data-ttu-id="4daa7-111">1</span><span class="sxs-lookup"><span data-stu-id="4daa7-111">1</span></span> | <span data-ttu-id="4daa7-112">1 GB (1 miljon händelser)</span><span class="sxs-lookup"><span data-stu-id="4daa7-112">1 GB (1 million events)</span></span> | <span data-ttu-id="4daa7-113">30 GB (30 miljoner händelser) per månad</span><span class="sxs-lookup"><span data-stu-id="4daa7-113">30 GB (30 million events) per month</span></span> |
| <span data-ttu-id="4daa7-114">10</span><span class="sxs-lookup"><span data-stu-id="4daa7-114">10</span></span> | <span data-ttu-id="4daa7-115">10 GB (10 miljoner händelser)</span><span class="sxs-lookup"><span data-stu-id="4daa7-115">10 GB (10 million events)</span></span> | <span data-ttu-id="4daa7-116">300 GB (300 miljoner händelser) per månad</span><span class="sxs-lookup"><span data-stu-id="4daa7-116">300 GB (300 million events) per month</span></span> |

## <a name="s2-sku-ingress-rates-and-capacities"></a><span data-ttu-id="4daa7-117">S2 SKU ingång priser och kapacitet</span><span class="sxs-lookup"><span data-stu-id="4daa7-117">S2 SKU ingress rates and capacities</span></span>

| <span data-ttu-id="4daa7-118">S2 Artikelnummerkapaciteten</span><span class="sxs-lookup"><span data-stu-id="4daa7-118">S2 SKU Capacity</span></span> | <span data-ttu-id="4daa7-119">Ingång hastighet</span><span class="sxs-lookup"><span data-stu-id="4daa7-119">Ingress Rate</span></span> | <span data-ttu-id="4daa7-120">Maximal lagringskapacitet</span><span class="sxs-lookup"><span data-stu-id="4daa7-120">Maximum Storage Capacity</span></span>
| --- | --- | --- |
| <span data-ttu-id="4daa7-121">1</span><span class="sxs-lookup"><span data-stu-id="4daa7-121">1</span></span> | <span data-ttu-id="4daa7-122">10 GB (10 miljoner händelser)</span><span class="sxs-lookup"><span data-stu-id="4daa7-122">10 GB (10 million events)</span></span> | <span data-ttu-id="4daa7-123">300 GB (300 miljoner händelser) per månad</span><span class="sxs-lookup"><span data-stu-id="4daa7-123">300 GB (300 million events) per month</span></span> |
| <span data-ttu-id="4daa7-124">10</span><span class="sxs-lookup"><span data-stu-id="4daa7-124">10</span></span> | <span data-ttu-id="4daa7-125">100 GB (100 miljoner händelser)</span><span class="sxs-lookup"><span data-stu-id="4daa7-125">100 GB (100 million events)</span></span> | <span data-ttu-id="4daa7-126">3 TB (3 miljarder händelser) per månad</span><span class="sxs-lookup"><span data-stu-id="4daa7-126">3 TB (3 billion events) per month</span></span> |

<span data-ttu-id="4daa7-127">Kapaciteter skalas linjärt, så en S1 sku med kapacitet 2 stöder 2 GB (2 miljoner) händelser per dag ingång hastighet och 60 GB (60 miljoner händelser) per månad.</span><span class="sxs-lookup"><span data-stu-id="4daa7-127">Capacities scale linearly, so a S1 sku with capacity 2 supports 2 GB (2 million) events per day ingress rate and 60 GB (60 million events) per month.</span></span>

## <a name="changing-the-capacity-of-your-environment"></a><span data-ttu-id="4daa7-128">Ändra kapaciteten för din miljö</span><span class="sxs-lookup"><span data-stu-id="4daa7-128">Changing the capacity of your environment</span></span>

1. <span data-ttu-id="4daa7-129">Välj miljön vars kapacitet du vill ändra i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4daa7-129">In the Azure portal, select the environment whose capacity you want to change.</span></span>
1. <span data-ttu-id="4daa7-130">Klicka på Konfigurera under inställningar.</span><span class="sxs-lookup"><span data-stu-id="4daa7-130">Under Settings, click Configure.</span></span>
1. <span data-ttu-id="4daa7-131">Använd skjutreglaget kapacitet för att välja den kapacitet som uppfyller kraven för meddelanden om ingångs-priser och lagringskapacitet.</span><span class="sxs-lookup"><span data-stu-id="4daa7-131">Use the Capacity slider to select the capacity that meets the requirements for your ingress rates and storage capacity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4daa7-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4daa7-132">Next steps</span></span>

* <span data-ttu-id="4daa7-133">Kontrollera att den nya kapaciteten är tillräckliga för att förhindra begränsning.</span><span class="sxs-lookup"><span data-stu-id="4daa7-133">Verify that the new capacity is sufficient to prevent throttling.</span></span> <span data-ttu-id="4daa7-134">Mer information finns i *din miljö kan att komma begränsas* avsnittet [här](time-series-insights-diagnose-and-solve-problems.md).</span><span class="sxs-lookup"><span data-stu-id="4daa7-134">For more details, see the *Your environment might be getting throttled* section [here](time-series-insights-diagnose-and-solve-problems.md).</span></span>