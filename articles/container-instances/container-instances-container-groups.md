---
title: "Azure-behållaren instanser Behållargrupper"
description: "Förstå hur Behållargrupper fungerar i Azure Container instanser"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 25eab41c3f0c986bcce33123f86f4c9638b77191
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="container-groups-in-azure-container-instances"></a><span data-ttu-id="05416-103">Behållargrupper i Azure-Behållarinstanser</span><span class="sxs-lookup"><span data-stu-id="05416-103">Container Groups in Azure Container Instances</span></span>

<span data-ttu-id="05416-104">Resursen på den översta nivån i Azure Container instanser är en grupp i behållaren.</span><span class="sxs-lookup"><span data-stu-id="05416-104">The top-level resource in Azure Container Instances is a container group.</span></span> <span data-ttu-id="05416-105">Den här artikeln beskriver vad behållargrupper är och vilka typer av scenarier som de aktiverar.</span><span class="sxs-lookup"><span data-stu-id="05416-105">This article describes what container groups are and what types of scenarios they enable.</span></span>

## <a name="how-a-container-group-works"></a><span data-ttu-id="05416-106">Så här fungerar en behållare grupp</span><span class="sxs-lookup"><span data-stu-id="05416-106">How a container group works</span></span>

<span data-ttu-id="05416-107">En behållare grupp är en samling av behållare som hämta schemaläggs på samma värddator och dela en livscykel, lokala nätverk och lagringsvolymer.</span><span class="sxs-lookup"><span data-stu-id="05416-107">A container group is a collection of containers that get scheduled on the same host machine and share a lifecycle, local network, and storage volumes.</span></span> <span data-ttu-id="05416-108">Den liknar konceptet för en *baljor* i [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) och [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).</span><span class="sxs-lookup"><span data-stu-id="05416-108">It is similar to the concept of a *pod* in [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) and [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).</span></span>

<span data-ttu-id="05416-109">Följande diagram visar ett exempel på en behållare grupp som innehåller flera behållare.</span><span class="sxs-lookup"><span data-stu-id="05416-109">The following diagram shows an example of a container group that includes multiple containers.</span></span>

![Behållaren grupper exempel][container-groups-example]

<span data-ttu-id="05416-111">Tänk på följande:</span><span class="sxs-lookup"><span data-stu-id="05416-111">Note that:</span></span>

- <span data-ttu-id="05416-112">Gruppen har schemalagts på en enda värddator.</span><span class="sxs-lookup"><span data-stu-id="05416-112">The group is scheduled on a single host machine.</span></span>
- <span data-ttu-id="05416-113">Gruppen Exponerar en offentlig IP-adress, med en exponerade port.</span><span class="sxs-lookup"><span data-stu-id="05416-113">The group exposes a single public IP address, with one exposed port.</span></span>
- <span data-ttu-id="05416-114">Gruppen består av två behållare.</span><span class="sxs-lookup"><span data-stu-id="05416-114">The group is made up of two containers.</span></span> <span data-ttu-id="05416-115">En behållare lyssnar på port 80, medan andra lyssnar på port 5000.</span><span class="sxs-lookup"><span data-stu-id="05416-115">One container listens on port 80, while the other listens on port 5000.</span></span>
- <span data-ttu-id="05416-116">Gruppen innehåller två Azure-filresurser som volym monteringar och varje behållare monterar en resurs lokalt.</span><span class="sxs-lookup"><span data-stu-id="05416-116">The group includes two Azure file shares as volume mounts, and each container mounts one of the shares locally.</span></span>

### <a name="networking"></a><span data-ttu-id="05416-117">Nätverk</span><span class="sxs-lookup"><span data-stu-id="05416-117">Networking</span></span>

<span data-ttu-id="05416-118">Behållargrupper delar en IP-adress och ett namnområde för port på att IP-adress.</span><span class="sxs-lookup"><span data-stu-id="05416-118">Container groups share an IP address and a port namespace on that IP address.</span></span> <span data-ttu-id="05416-119">Om du vill aktivera externa klienter ska nå en behållare i gruppen, måste du exponera port på IP-adressen och från behållaren.</span><span class="sxs-lookup"><span data-stu-id="05416-119">To enable external clients to reach a container within the group, you must expose the port on the IP address and from the container.</span></span> <span data-ttu-id="05416-120">Eftersom behållare i gruppen delar port namnområde stöds portmappning inte.</span><span class="sxs-lookup"><span data-stu-id="05416-120">Because containers within the group share a port namespace, port mapping is not supported.</span></span> <span data-ttu-id="05416-121">Behållare i en grupp kan du nå en varandra via localhost på de portar som de har exponerade, även om dessa portar inte är tillgängliga externt på gruppens IP-adress.</span><span class="sxs-lookup"><span data-stu-id="05416-121">Containers within a group can reach each other via localhost on the ports that they have exposed, even if those ports are not exposed externally on the group's IP address.</span></span>

### <a name="storage"></a><span data-ttu-id="05416-122">Lagring</span><span class="sxs-lookup"><span data-stu-id="05416-122">Storage</span></span>

<span data-ttu-id="05416-123">Du kan ange externa volymer att montera i en grupp i behållaren.</span><span class="sxs-lookup"><span data-stu-id="05416-123">You can specify external volumes to mount within a container group.</span></span> <span data-ttu-id="05416-124">Du kan mappa volymerna till specifika sökvägar i enskilda behållare i en grupp.</span><span class="sxs-lookup"><span data-stu-id="05416-124">You can map those volumes into specific paths within the individual containers in a group.</span></span>

## <a name="common-scenarios"></a><span data-ttu-id="05416-125">Vanliga scenarier</span><span class="sxs-lookup"><span data-stu-id="05416-125">Common scenarios</span></span>

<span data-ttu-id="05416-126">Flera behållare grupper är användbart i fall där du vill dela upp en funktionell aktivitet i ett litet antal behållare bilder, som levereras med olika team och ha separata resurskraven.</span><span class="sxs-lookup"><span data-stu-id="05416-126">Multi-container groups are useful in cases where you want to divide up a single functional task into a small number of container images, which can be delivered by different teams and have separate resource requirements.</span></span> <span data-ttu-id="05416-127">Exempel på användning kan vara:</span><span class="sxs-lookup"><span data-stu-id="05416-127">Example usage could include:</span></span>

- <span data-ttu-id="05416-128">En behållare för program och en behållare för loggning.</span><span class="sxs-lookup"><span data-stu-id="05416-128">An application container and a logging container.</span></span> <span data-ttu-id="05416-129">Behållaren loggning samlar in loggar och mått utdata av de huvudsakliga programmet och skriver dem till långsiktig lagring.</span><span class="sxs-lookup"><span data-stu-id="05416-129">The logging container collects the logs and metrics output by the main application and writes them to long-term storage.</span></span>
- <span data-ttu-id="05416-130">Ett program och en behållare för övervakning.</span><span class="sxs-lookup"><span data-stu-id="05416-130">An application and a monitoring container.</span></span> <span data-ttu-id="05416-131">Behållaren övervakning skickar med jämna mellanrum en begäran till programmet så att den körs och svara på rätt sätt och aktiverar en avisering om det inte.</span><span class="sxs-lookup"><span data-stu-id="05416-131">The monitoring container periodically makes a request to the application to ensure that it's running and responding correctly and raises an alert if it's not.</span></span>
- <span data-ttu-id="05416-132">En behållare som betjänar ett webbprogram och en behållare dra det senaste innehållet från källkontroll.</span><span class="sxs-lookup"><span data-stu-id="05416-132">A container serving a web application and a container pulling the latest content from source control.</span></span>

## <a name="next-steps"></a><span data-ttu-id="05416-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="05416-133">Next steps</span></span>

<span data-ttu-id="05416-134">Lär dig hur du [distribuera flera behållargruppen](container-instances-multi-container-group.md) med en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="05416-134">Learn how to [deploy a multi-container group](container-instances-multi-container-group.md) with an Azure Resource Manager template.</span></span>

<!-- IMAGES -->

[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png