---
title: "aaaHow tooscale Azure tid serien Insights miljön | Microsoft Docs"
description: "Den här självstudiekursen beskrivs hur tooscale Azure tid serien Insights-miljö"
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
ms.openlocfilehash: 55eda388997589185bd34228762b95e182b228ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-your-time-series-insights-environment"></a><span data-ttu-id="7c173-103">Hur tooscale tid serien insikter miljön</span><span class="sxs-lookup"><span data-stu-id="7c173-103">How tooscale your Time Series Insights environment</span></span>

<span data-ttu-id="7c173-104">Den här självstudiekursen beskrivs hur tooscale tid serien insikter miljön.</span><span class="sxs-lookup"><span data-stu-id="7c173-104">This tutorial covers how tooscale your Time Series Insights environment.</span></span>

> [!NOTE]
> <span data-ttu-id="7c173-105">Skala upp över sku-typer tillåts inte.</span><span class="sxs-lookup"><span data-stu-id="7c173-105">Scale up across sku types is not allowed.</span></span> <span data-ttu-id="7c173-106">En miljö med en S1 Sku kan inte konverteras till en S2 miljö.</span><span class="sxs-lookup"><span data-stu-id="7c173-106">An environment with a S1 Sku cannot be converted into an S2 environment.</span></span>

## <a name="s1-sku-ingress-rates-and-capacities"></a><span data-ttu-id="7c173-107">S1-SKU ingång priser och kapacitet</span><span class="sxs-lookup"><span data-stu-id="7c173-107">S1 SKU ingress rates and capacities</span></span>

| <span data-ttu-id="7c173-108">S1 Artikelnummerkapaciteten</span><span class="sxs-lookup"><span data-stu-id="7c173-108">S1 SKU Capacity</span></span> | <span data-ttu-id="7c173-109">Ingång hastighet</span><span class="sxs-lookup"><span data-stu-id="7c173-109">Ingress Rate</span></span> | <span data-ttu-id="7c173-110">Maximal lagringskapacitet</span><span class="sxs-lookup"><span data-stu-id="7c173-110">Maximum Storage Capacity</span></span>
| --- | --- | --- |
| <span data-ttu-id="7c173-111">1</span><span class="sxs-lookup"><span data-stu-id="7c173-111">1</span></span> | <span data-ttu-id="7c173-112">1 GB (1 miljon händelser)</span><span class="sxs-lookup"><span data-stu-id="7c173-112">1 GB (1 million events)</span></span> | <span data-ttu-id="7c173-113">30 GB (30 miljoner händelser) per månad</span><span class="sxs-lookup"><span data-stu-id="7c173-113">30 GB (30 million events) per month</span></span> |
| <span data-ttu-id="7c173-114">10</span><span class="sxs-lookup"><span data-stu-id="7c173-114">10</span></span> | <span data-ttu-id="7c173-115">10 GB (10 miljoner händelser)</span><span class="sxs-lookup"><span data-stu-id="7c173-115">10 GB (10 million events)</span></span> | <span data-ttu-id="7c173-116">300 GB (300 miljoner händelser) per månad</span><span class="sxs-lookup"><span data-stu-id="7c173-116">300 GB (300 million events) per month</span></span> |

## <a name="s2-sku-ingress-rates-and-capacities"></a><span data-ttu-id="7c173-117">S2 SKU ingång priser och kapacitet</span><span class="sxs-lookup"><span data-stu-id="7c173-117">S2 SKU ingress rates and capacities</span></span>

| <span data-ttu-id="7c173-118">S2 Artikelnummerkapaciteten</span><span class="sxs-lookup"><span data-stu-id="7c173-118">S2 SKU Capacity</span></span> | <span data-ttu-id="7c173-119">Ingång hastighet</span><span class="sxs-lookup"><span data-stu-id="7c173-119">Ingress Rate</span></span> | <span data-ttu-id="7c173-120">Maximal lagringskapacitet</span><span class="sxs-lookup"><span data-stu-id="7c173-120">Maximum Storage Capacity</span></span>
| --- | --- | --- |
| <span data-ttu-id="7c173-121">1</span><span class="sxs-lookup"><span data-stu-id="7c173-121">1</span></span> | <span data-ttu-id="7c173-122">10 GB (10 miljoner händelser)</span><span class="sxs-lookup"><span data-stu-id="7c173-122">10 GB (10 million events)</span></span> | <span data-ttu-id="7c173-123">300 GB (300 miljoner händelser) per månad</span><span class="sxs-lookup"><span data-stu-id="7c173-123">300 GB (300 million events) per month</span></span> |
| <span data-ttu-id="7c173-124">10</span><span class="sxs-lookup"><span data-stu-id="7c173-124">10</span></span> | <span data-ttu-id="7c173-125">100 GB (100 miljoner händelser)</span><span class="sxs-lookup"><span data-stu-id="7c173-125">100 GB (100 million events)</span></span> | <span data-ttu-id="7c173-126">3 TB (3 miljarder händelser) per månad</span><span class="sxs-lookup"><span data-stu-id="7c173-126">3 TB (3 billion events) per month</span></span> |

<span data-ttu-id="7c173-127">Kapaciteter skalas linjärt, så en S1 sku med kapacitet 2 stöder 2 GB (2 miljoner) händelser per dag ingång hastighet och 60 GB (60 miljoner händelser) per månad.</span><span class="sxs-lookup"><span data-stu-id="7c173-127">Capacities scale linearly, so a S1 sku with capacity 2 supports 2 GB (2 million) events per day ingress rate and 60 GB (60 million events) per month.</span></span>

## <a name="changing-hello-capacity-of-your-environment"></a><span data-ttu-id="7c173-128">Ändra hello kapaciteten för din miljö</span><span class="sxs-lookup"><span data-stu-id="7c173-128">Changing hello capacity of your environment</span></span>

1. <span data-ttu-id="7c173-129">I hello Azure-portalen, Välj hello miljö vars kapacitet du vill toochange.</span><span class="sxs-lookup"><span data-stu-id="7c173-129">In hello Azure portal, select hello environment whose capacity you want toochange.</span></span>
1. <span data-ttu-id="7c173-130">Klicka på Konfigurera under inställningar.</span><span class="sxs-lookup"><span data-stu-id="7c173-130">Under Settings, click Configure.</span></span>
1. <span data-ttu-id="7c173-131">Använd hello kapacitet skjutreglaget tooselect hello kapaciteten som uppfyller hello för meddelanden om ingångs-priser och lagringskapacitet.</span><span class="sxs-lookup"><span data-stu-id="7c173-131">Use hello Capacity slider tooselect hello capacity that meets hello requirements for your ingress rates and storage capacity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7c173-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c173-132">Next steps</span></span>

* <span data-ttu-id="7c173-133">Kontrollera att det räcker hello ny kapacitet tooprevent begränsning.</span><span class="sxs-lookup"><span data-stu-id="7c173-133">Verify that hello new capacity is sufficient tooprevent throttling.</span></span> <span data-ttu-id="7c173-134">Mer information finns i hello *din miljö kan att komma begränsas* avsnittet [här](time-series-insights-diagnose-and-solve-problems.md).</span><span class="sxs-lookup"><span data-stu-id="7c173-134">For more details, see hello *Your environment might be getting throttled* section [here](time-series-insights-diagnose-and-solve-problems.md).</span></span>
