---
title: "Översikt över vanliga Autoskala mönster | Microsoft Docs"
description: "Läs om några av de vanliga mönster för att automatiskt skala din resurs i Azure."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: ancav
ms.openlocfilehash: fce51546e041c8989d813c3935e058c52b38ba77
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="overview-of-common-autoscale-patterns"></a><span data-ttu-id="e2dbb-103">Översikt över vanliga Autoskala mönster</span><span class="sxs-lookup"><span data-stu-id="e2dbb-103">Overview of common autoscale patterns</span></span>
<span data-ttu-id="e2dbb-104">Den här artikeln beskrivs några vanliga mönster för att skala din resurs i Azure.</span><span class="sxs-lookup"><span data-stu-id="e2dbb-104">This article describes some of the common patterns to scale your resource in Azure.</span></span>

<span data-ttu-id="e2dbb-105">Azure övervakaren Autoskala gäller enbart för Virtual Machine Scale uppsättningar (VMSS), cloud services, apptjänstplaner och apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="e2dbb-105">Azure Monitor auto scale applies only to Virtual Machine Scale Sets (VMSS), cloud services, app service plans and app service environments.</span></span> 

# <a name="lets-get-started"></a><span data-ttu-id="e2dbb-106">Kan komma igång</span><span class="sxs-lookup"><span data-stu-id="e2dbb-106">Lets get started</span></span>

<span data-ttu-id="e2dbb-107">Den här artikeln förutsätter att du är bekant med automatisk skalning.</span><span class="sxs-lookup"><span data-stu-id="e2dbb-107">This article assumes that you are familiar with auto scale.</span></span> <span data-ttu-id="e2dbb-108">Du kan [starta här att skala din resurs][1].</span><span class="sxs-lookup"><span data-stu-id="e2dbb-108">You can [get started here to scale your resource][1].</span></span> <span data-ttu-id="e2dbb-109">Följande är några av de vanliga skala mönster.</span><span class="sxs-lookup"><span data-stu-id="e2dbb-109">The following are some of the common scale patterns.</span></span>

## <a name="scale-based-on-cpu"></a><span data-ttu-id="e2dbb-110">Skala baserat på CPU</span><span class="sxs-lookup"><span data-stu-id="e2dbb-110">Scale based on CPU</span></span>

<span data-ttu-id="e2dbb-111">Du har ett webbprogram (/ VMSS/molnet rolltjänst) och</span><span class="sxs-lookup"><span data-stu-id="e2dbb-111">You have a web app (/VMSS/cloud service role) and</span></span> 

- <span data-ttu-id="e2dbb-112">Du vill skala ut/skala i baserat på CPU.</span><span class="sxs-lookup"><span data-stu-id="e2dbb-112">You want to scale out/scale in based on CPU.</span></span>
- <span data-ttu-id="e2dbb-113">Dessutom kan vill du se till att det finns ett minsta antal instanser.</span><span class="sxs-lookup"><span data-stu-id="e2dbb-113">Additionally, you want to ensure there is a minimum number of instances.</span></span> 
- <span data-ttu-id="e2dbb-114">Du också se till att du anger en övre gräns för antalet instanser som kan skalas till.</span><span class="sxs-lookup"><span data-stu-id="e2dbb-114">Also, you want to ensure that you set a maximum limit to the number of instances you can scale to.</span></span>

![Skala baserat på CPU][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a><span data-ttu-id="e2dbb-116">Skala annorlunda veckodagar vs helger</span><span class="sxs-lookup"><span data-stu-id="e2dbb-116">Scale differently on weekdays vs weekends</span></span>

<span data-ttu-id="e2dbb-117">Du har ett webbprogram (/ VMSS/molnet rolltjänst) och</span><span class="sxs-lookup"><span data-stu-id="e2dbb-117">You have a web app (/VMSS/cloud service role) and</span></span>

- <span data-ttu-id="e2dbb-118">Du vill 3 instanser som standard (på vardagar)</span><span class="sxs-lookup"><span data-stu-id="e2dbb-118">You want 3 instances by default (on weekdays)</span></span>
- <span data-ttu-id="e2dbb-119">Du förväntar dig inte trafik på helger och därför du vill skala ned 1 instans på helger.</span><span class="sxs-lookup"><span data-stu-id="e2dbb-119">You don't expect traffic on weekends and hence you want to scale down to 1 instance on weekends.</span></span>

![Skala annorlunda veckodagar vs helger][3]

## <a name="scale-differently-during-holidays"></a><span data-ttu-id="e2dbb-121">Skala annorlunda under helgdagar</span><span class="sxs-lookup"><span data-stu-id="e2dbb-121">Scale differently during holidays</span></span>

<span data-ttu-id="e2dbb-122">Du har ett webbprogram (/ VMSS/molnet rolltjänst) och</span><span class="sxs-lookup"><span data-stu-id="e2dbb-122">You have a web app (/VMSS/cloud service role) and</span></span> 

- <span data-ttu-id="e2dbb-123">Du vill skala upp/ned baserat på CPU-användning som standard</span><span class="sxs-lookup"><span data-stu-id="e2dbb-123">You want to scale up/down based on CPU usage by default</span></span>
- <span data-ttu-id="e2dbb-124">Dock under köpstarka jul (eller särskilda dagar som är viktiga för ditt företag) vill du åsidosätta standardvärdena och dina anställda har mer kapacitet.</span><span class="sxs-lookup"><span data-stu-id="e2dbb-124">However, during holiday season (or specific days that are important for your business) you want to override the defaults and have more capacity at your disposal.</span></span>

![Skala annorlunda på helgdagar][4]

## <a name="scale-based-on-custom-metric"></a><span data-ttu-id="e2dbb-126">Skala baserat på anpassade mått</span><span class="sxs-lookup"><span data-stu-id="e2dbb-126">Scale based on custom metric</span></span>

<span data-ttu-id="e2dbb-127">Du har en frontwebb och en API-nivå som kommunicerar med serverdelen.</span><span class="sxs-lookup"><span data-stu-id="e2dbb-127">You have a web front end and a API tier that communicates with the backend.</span></span> 

- <span data-ttu-id="e2dbb-128">Du vill skala API-nivå som baseras på anpassade händelser i klientdelen (exempel: du vill skala utcheckningen processen baserat på antalet objekt i kundvagnen)</span><span class="sxs-lookup"><span data-stu-id="e2dbb-128">You want to scale the API tier based on custom events in the front end (example: You want to scale your checkout process based on the number of items in the shopping cart)</span></span>

![Skala baserat på anpassade mått][5]

<!--Reference-->
[1]: ./monitoring-autoscale-get-started.md
[2]: ./media/monitoring-autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/monitoring-autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/monitoring-autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/monitoring-autoscale-common-scale-patterns/custom-metric-scale.png