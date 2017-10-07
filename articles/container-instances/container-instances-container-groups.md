---
title: "aaaAzure Behållargrupper instanser behållare"
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
ms.openlocfilehash: 2b0e784609979045c8f77d7b6d0987ec3fee714c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="container-groups-in-azure-container-instances"></a><span data-ttu-id="b455f-103">Behållargrupper i Azure-Behållarinstanser</span><span class="sxs-lookup"><span data-stu-id="b455f-103">Container Groups in Azure Container Instances</span></span>

<span data-ttu-id="b455f-104">hello översta resurs i Azure Container instanser är en grupp i behållaren.</span><span class="sxs-lookup"><span data-stu-id="b455f-104">hello top-level resource in Azure Container Instances is a container group.</span></span> <span data-ttu-id="b455f-105">Den här artikeln beskriver vad behållargrupper är och vilka typer av scenarier som de aktiverar.</span><span class="sxs-lookup"><span data-stu-id="b455f-105">This article describes what container groups are and what types of scenarios they enable.</span></span>

## <a name="how-a-container-group-works"></a><span data-ttu-id="b455f-106">Så här fungerar en behållare grupp</span><span class="sxs-lookup"><span data-stu-id="b455f-106">How a container group works</span></span>

<span data-ttu-id="b455f-107">En behållare är en samling av behållare som få schemalagda på hello samma värd för dator och dela en livscykel, lokala nätverk och lagringsvolymer.</span><span class="sxs-lookup"><span data-stu-id="b455f-107">A container group is a collection of containers that get scheduled on hello same host machine and share a lifecycle, local network, and storage volumes.</span></span> <span data-ttu-id="b455f-108">Det är liknande toohello begreppet en *baljor* i [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) och [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).</span><span class="sxs-lookup"><span data-stu-id="b455f-108">It is similar toohello concept of a *pod* in [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) and [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).</span></span>

<span data-ttu-id="b455f-109">hello visar följande diagram ett exempel på en behållare grupp som innehåller flera behållare.</span><span class="sxs-lookup"><span data-stu-id="b455f-109">hello following diagram shows an example of a container group that includes multiple containers.</span></span>

![Behållaren grupper exempel][container-groups-example]

<span data-ttu-id="b455f-111">Tänk på följande:</span><span class="sxs-lookup"><span data-stu-id="b455f-111">Note that:</span></span>

- <span data-ttu-id="b455f-112">hello grupp schemaläggs på en enda värddator.</span><span class="sxs-lookup"><span data-stu-id="b455f-112">hello group is scheduled on a single host machine.</span></span>
- <span data-ttu-id="b455f-113">hello grupp Exponerar en offentlig IP-adress, med en exponerade port.</span><span class="sxs-lookup"><span data-stu-id="b455f-113">hello group exposes a single public IP address, with one exposed port.</span></span>
- <span data-ttu-id="b455f-114">hello-grupp består av två behållare.</span><span class="sxs-lookup"><span data-stu-id="b455f-114">hello group is made up of two containers.</span></span> <span data-ttu-id="b455f-115">En behållare lyssnar på port 80, medan andra hello lyssnar på port 5000.</span><span class="sxs-lookup"><span data-stu-id="b455f-115">One container listens on port 80, while hello other listens on port 5000.</span></span>
- <span data-ttu-id="b455f-116">hello grupp innehåller två Azure-filresurser som volym monteringar, och varje behållare monterar en hello resurser lokalt.</span><span class="sxs-lookup"><span data-stu-id="b455f-116">hello group includes two Azure file shares as volume mounts, and each container mounts one of hello shares locally.</span></span>

### <a name="networking"></a><span data-ttu-id="b455f-117">Nätverk</span><span class="sxs-lookup"><span data-stu-id="b455f-117">Networking</span></span>

<span data-ttu-id="b455f-118">Behållargrupper delar en IP-adress och ett namnområde för port på att IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b455f-118">Container groups share an IP address and a port namespace on that IP address.</span></span> <span data-ttu-id="b455f-119">tooenable externa klienter tooreach en behållare i hello grupp, måste du exponera hello port på hello IP-adress och från hello behållare.</span><span class="sxs-lookup"><span data-stu-id="b455f-119">tooenable external clients tooreach a container within hello group, you must expose hello port on hello IP address and from hello container.</span></span> <span data-ttu-id="b455f-120">Eftersom behållare i hello grupp delar port namnområde stöds portmappning inte.</span><span class="sxs-lookup"><span data-stu-id="b455f-120">Because containers within hello group share a port namespace, port mapping is not supported.</span></span> <span data-ttu-id="b455f-121">Behållare i en grupp kan nå varandra via localhost på hello portar som de har exponerade, även om dessa portar inte är tillgängliga externt på hello gruppens IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b455f-121">Containers within a group can reach each other via localhost on hello ports that they have exposed, even if those ports are not exposed externally on hello group's IP address.</span></span>

### <a name="storage"></a><span data-ttu-id="b455f-122">Lagring</span><span class="sxs-lookup"><span data-stu-id="b455f-122">Storage</span></span>

<span data-ttu-id="b455f-123">Du kan ange externa volymer toomount i en grupp i behållaren.</span><span class="sxs-lookup"><span data-stu-id="b455f-123">You can specify external volumes toomount within a container group.</span></span> <span data-ttu-id="b455f-124">Du kan mappa volymerna till specifika sökvägar i hello enskilda behållare i en grupp.</span><span class="sxs-lookup"><span data-stu-id="b455f-124">You can map those volumes into specific paths within hello individual containers in a group.</span></span>

## <a name="common-scenarios"></a><span data-ttu-id="b455f-125">Vanliga scenarier</span><span class="sxs-lookup"><span data-stu-id="b455f-125">Common scenarios</span></span>

<span data-ttu-id="b455f-126">Flera behållare grupper är användbart i fall där du vill toodivide en enda funktionella uppgift i ett litet antal behållare bilder, som levereras med olika team och ha separata resurskraven.</span><span class="sxs-lookup"><span data-stu-id="b455f-126">Multi-container groups are useful in cases where you want toodivide up a single functional task into a small number of container images, which can be delivered by different teams and have separate resource requirements.</span></span> <span data-ttu-id="b455f-127">Exempel på användning kan vara:</span><span class="sxs-lookup"><span data-stu-id="b455f-127">Example usage could include:</span></span>

- <span data-ttu-id="b455f-128">En behållare för program och en behållare för loggning.</span><span class="sxs-lookup"><span data-stu-id="b455f-128">An application container and a logging container.</span></span> <span data-ttu-id="b455f-129">hello loggning behållaren samlar in hello loggar och mått utdata med hello programmets och skriver dem toolong-lagring.</span><span class="sxs-lookup"><span data-stu-id="b455f-129">hello logging container collects hello logs and metrics output by hello main application and writes them toolong-term storage.</span></span>
- <span data-ttu-id="b455f-130">Ett program och en behållare för övervakning.</span><span class="sxs-lookup"><span data-stu-id="b455f-130">An application and a monitoring container.</span></span> <span data-ttu-id="b455f-131">hello övervakning behållaren regelbundet gör en begäran toohello programmet tooensure att den körs och svara på rätt sätt och aktiverar en avisering om det inte.</span><span class="sxs-lookup"><span data-stu-id="b455f-131">hello monitoring container periodically makes a request toohello application tooensure that it's running and responding correctly and raises an alert if it's not.</span></span>
- <span data-ttu-id="b455f-132">En behållare som betjänar ett webbprogram och en behållare dra hello senaste innehållet från källkontroll.</span><span class="sxs-lookup"><span data-stu-id="b455f-132">A container serving a web application and a container pulling hello latest content from source control.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b455f-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b455f-133">Next steps</span></span>

<span data-ttu-id="b455f-134">Lär dig hur för[distribuera flera behållargruppen](container-instances-multi-container-group.md) med en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="b455f-134">Learn how too[deploy a multi-container group](container-instances-multi-container-group.md) with an Azure Resource Manager template.</span></span>

<!-- IMAGES -->

[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png