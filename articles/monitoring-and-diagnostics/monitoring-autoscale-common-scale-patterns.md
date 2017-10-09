---
title: "aaaOverview av vanliga Autoskala mönster | Microsoft Docs"
description: "Läs om några av hello vanliga mönster tooauto skala din resurs i Azure."
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
ms.openlocfilehash: fc5bd97852e0af01aa32940c99721ab8e21033ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-common-autoscale-patterns"></a><span data-ttu-id="d22ae-103">Översikt över vanliga Autoskala mönster</span><span class="sxs-lookup"><span data-stu-id="d22ae-103">Overview of common autoscale patterns</span></span>
<span data-ttu-id="d22ae-104">Den här artikeln beskrivs några av hello vanliga mönster tooscale resurs i Azure.</span><span class="sxs-lookup"><span data-stu-id="d22ae-104">This article describes some of hello common patterns tooscale your resource in Azure.</span></span>

<span data-ttu-id="d22ae-105">Azure övervakaren Autoskala gäller endast tooVirtual datorn skala uppsättningar (VMSS), cloud services, apptjänstplaner och apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="d22ae-105">Azure Monitor auto scale applies only tooVirtual Machine Scale Sets (VMSS), cloud services, app service plans and app service environments.</span></span> 

# <a name="lets-get-started"></a><span data-ttu-id="d22ae-106">Kan komma igång</span><span class="sxs-lookup"><span data-stu-id="d22ae-106">Lets get started</span></span>

<span data-ttu-id="d22ae-107">Den här artikeln förutsätter att du är bekant med automatisk skalning.</span><span class="sxs-lookup"><span data-stu-id="d22ae-107">This article assumes that you are familiar with auto scale.</span></span> <span data-ttu-id="d22ae-108">Du kan [Kom igång här tooscale resurs][1].</span><span class="sxs-lookup"><span data-stu-id="d22ae-108">You can [get started here tooscale your resource][1].</span></span> <span data-ttu-id="d22ae-109">hello följande är några av hello vanliga skala mönster.</span><span class="sxs-lookup"><span data-stu-id="d22ae-109">hello following are some of hello common scale patterns.</span></span>

## <a name="scale-based-on-cpu"></a><span data-ttu-id="d22ae-110">Skala baserat på CPU</span><span class="sxs-lookup"><span data-stu-id="d22ae-110">Scale based on CPU</span></span>

<span data-ttu-id="d22ae-111">Du har ett webbprogram (/ VMSS/molnet rolltjänst) och</span><span class="sxs-lookup"><span data-stu-id="d22ae-111">You have a web app (/VMSS/cloud service role) and</span></span> 

- <span data-ttu-id="d22ae-112">Du vill tooscale out/skala i baserat på CPU.</span><span class="sxs-lookup"><span data-stu-id="d22ae-112">You want tooscale out/scale in based on CPU.</span></span>
- <span data-ttu-id="d22ae-113">Dessutom kan du vill tooensure det finns ett minsta antal instanser.</span><span class="sxs-lookup"><span data-stu-id="d22ae-113">Additionally, you want tooensure there is a minimum number of instances.</span></span> 
- <span data-ttu-id="d22ae-114">Du bör också, tooensure som du anger en övre gräns toohello antalet instanser som kan skalas till.</span><span class="sxs-lookup"><span data-stu-id="d22ae-114">Also, you want tooensure that you set a maximum limit toohello number of instances you can scale to.</span></span>

![Skala baserat på CPU][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a><span data-ttu-id="d22ae-116">Skala annorlunda veckodagar vs helger</span><span class="sxs-lookup"><span data-stu-id="d22ae-116">Scale differently on weekdays vs weekends</span></span>

<span data-ttu-id="d22ae-117">Du har ett webbprogram (/ VMSS/molnet rolltjänst) och</span><span class="sxs-lookup"><span data-stu-id="d22ae-117">You have a web app (/VMSS/cloud service role) and</span></span>

- <span data-ttu-id="d22ae-118">Du vill 3 instanser som standard (på vardagar)</span><span class="sxs-lookup"><span data-stu-id="d22ae-118">You want 3 instances by default (on weekdays)</span></span>
- <span data-ttu-id="d22ae-119">Du förväntar dig inte trafik på helger och därför du vill tooscale ned too1 instans på helger.</span><span class="sxs-lookup"><span data-stu-id="d22ae-119">You don't expect traffic on weekends and hence you want tooscale down too1 instance on weekends.</span></span>

![Skala annorlunda veckodagar vs helger][3]

## <a name="scale-differently-during-holidays"></a><span data-ttu-id="d22ae-121">Skala annorlunda under helgdagar</span><span class="sxs-lookup"><span data-stu-id="d22ae-121">Scale differently during holidays</span></span>

<span data-ttu-id="d22ae-122">Du har ett webbprogram (/ VMSS/molnet rolltjänst) och</span><span class="sxs-lookup"><span data-stu-id="d22ae-122">You have a web app (/VMSS/cloud service role) and</span></span> 

- <span data-ttu-id="d22ae-123">Du vill tooscale upp/ned baserat på CPU-användning som standard</span><span class="sxs-lookup"><span data-stu-id="d22ae-123">You want tooscale up/down based on CPU usage by default</span></span>
- <span data-ttu-id="d22ae-124">Dock under köpstarka jul (eller särskilda dagar som är viktiga för ditt företag) toooverride hello standardinställningar och när du har mer kapacitet till din förfogande.</span><span class="sxs-lookup"><span data-stu-id="d22ae-124">However, during holiday season (or specific days that are important for your business) you want toooverride hello defaults and have more capacity at your disposal.</span></span>

![Skala annorlunda på helgdagar][4]

## <a name="scale-based-on-custom-metric"></a><span data-ttu-id="d22ae-126">Skala baserat på anpassade mått</span><span class="sxs-lookup"><span data-stu-id="d22ae-126">Scale based on custom metric</span></span>

<span data-ttu-id="d22ae-127">Du har en frontwebb och en API-nivå som kommunicerar med hello backend.</span><span class="sxs-lookup"><span data-stu-id="d22ae-127">You have a web front end and a API tier that communicates with hello backend.</span></span> 

- <span data-ttu-id="d22ae-128">Du vill tooscale hello API-nivå baserat på egna händelser i hello klientdelen (exempel: du vill tooscale utcheckningen processen baserat på hello antal objekt i hello kundvagn)</span><span class="sxs-lookup"><span data-stu-id="d22ae-128">You want tooscale hello API tier based on custom events in hello front end (example: You want tooscale your checkout process based on hello number of items in hello shopping cart)</span></span>

![Skala baserat på anpassade mått][5]

<!--Reference-->
[1]: ./monitoring-autoscale-get-started.md
[2]: ./media/monitoring-autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/monitoring-autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/monitoring-autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/monitoring-autoscale-common-scale-patterns/custom-metric-scale.png