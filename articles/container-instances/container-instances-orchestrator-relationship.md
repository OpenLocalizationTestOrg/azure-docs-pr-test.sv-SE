---
title: "aaaAzure Behållarinstanser och Orchestration-behållare"
description: "Förstå hur Azure Behållarinstanser interagera med behållaren orchestrators"
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
ms.date: 07/24/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 69a39edc6f14d885c1ac300990ed1399002ccfee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-instances-and-container-orchestrators"></a><span data-ttu-id="d6e1b-103">Azure Container instanser och behållare orchestrators</span><span class="sxs-lookup"><span data-stu-id="d6e1b-103">Azure Container Instances and container orchestrators</span></span>

<span data-ttu-id="d6e1b-104">Behållare är väl lämpade för flexibel leverans miljöer och mikrotjänster-baserade arkitekturer på grund av deras liten storlek och programmet orientering.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-104">Because of their small size and application orientation, containers are well suited for agile delivery environments and microservice-based architectures.</span></span> <span data-ttu-id="d6e1b-105">hello uppgiften att automatisera och hantera ett stort antal behållare och hur de samverkar kallas *orchestration*.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-105">hello task of automating and managing a large number of containers and how they interact is known as *orchestration*.</span></span> <span data-ttu-id="d6e1b-106">Populära behållaren orchestrators inkluderar Kubernetes DC/OS och Docker Swarm som är tillgängliga i hello [Azure Container Service](https://docs.microsoft.com/azure/container-service/).</span><span class="sxs-lookup"><span data-stu-id="d6e1b-106">Popular container orchestrators include Kubernetes, DC/OS, and Docker Swarm, all of which are available in hello [Azure Container Service](https://docs.microsoft.com/azure/container-service/).</span></span>

<span data-ttu-id="d6e1b-107">Behållarinstanser som Azure tillhandahåller en del av hello grundläggande funktionerna i orchestration-plattformar, men de täcker inte hello högre värde tjänster som dessa plattformar ger och kan i praktiken vara kompletterande med dem.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-107">Azure Container Instances provides some of hello basic scheduling capabilities of orchestration platforms, but it does not cover hello higher-value services that those platforms provide and can in fact be complementary with them.</span></span> <span data-ttu-id="d6e1b-108">Den här artikeln beskriver hello omfattning Azure Behållarinstanser hanterar och hur fullständig behållaren orchestrators kan interagera med den.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-108">This article describes hello scope of what Azure Container Instances handles and how full container orchestrators might interact with it.</span></span>

## <a name="traditional-orchestration"></a><span data-ttu-id="d6e1b-109">Traditionella orchestration</span><span class="sxs-lookup"><span data-stu-id="d6e1b-109">Traditional orchestration</span></span>

<span data-ttu-id="d6e1b-110">hello standard definition av orchestration omfattar hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="d6e1b-110">hello standard definition of orchestration includes hello following tasks:</span></span>

- <span data-ttu-id="d6e1b-111">**Schemalägga**: baserat på en avbildning av behållare och en resursbegäran om, hitta en lämplig dator på vilken toorun hello-behållare.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-111">**Scheduling**: Given a container image and a resource request, find a suitable machine on which toorun hello container.</span></span>
- <span data-ttu-id="d6e1b-112">**Tillhörighet/mot-affinity**: ange att en uppsättning behållare ska köras i närheten varandra (för prestanda) eller tillräckligt långt ifrån varandra (för tillgänglighet).</span><span class="sxs-lookup"><span data-stu-id="d6e1b-112">**Affinity/Anti-affinity**: Specify that a set of containers should run nearby each other (for performance) or sufficiently far apart (for availability).</span></span>
- <span data-ttu-id="d6e1b-113">**Hälsoövervakning**: Titta på för behållaren och automatiskt schemalägga dem.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-113">**Health monitoring**: Watch for container failures and automatically reschedule them.</span></span>
- <span data-ttu-id="d6e1b-114">**Redundans**: hålla reda på vad som körs på varje dator och schemalägga behållare från datorer som inte fungerar toohealthy noder.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-114">**Failover**: Keep track of what is running on each machine and reschedule containers from failed machines toohealthy nodes.</span></span>
- <span data-ttu-id="d6e1b-115">**Skalning**: Lägg till eller ta bort behållaren instanser toomatch begäran, antingen manuellt eller automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-115">**Scaling**: Add or remove container instances toomatch demand, either manually or automatically.</span></span>
- <span data-ttu-id="d6e1b-116">**Nätverk**: Ange ett överlägget nätverk för att koordinera behållare toocommunicate över flera datorer på värden.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-116">**Networking**: Provide an overlay network for coordinating containers toocommunicate across multiple host machines.</span></span>
- <span data-ttu-id="d6e1b-117">**Identifiering av tjänst**: aktivera behållare toolocate varandra automatiskt även när de flyttar mellan värddatorerna och ändra IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-117">**Service discovery**: Enable containers toolocate each other automatically even as they move between host machines and change IP addresses.</span></span>
- <span data-ttu-id="d6e1b-118">**Koordineras programuppgraderingar**: hantera uppgraderingar tooavoid programmet stillestånd och aktivera återställning om något går fel.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-118">**Coordinated application upgrades**: Manage container upgrades tooavoid application down time and enable rollback if something goes wrong.</span></span>

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a><span data-ttu-id="d6e1b-119">Orchestration med Azure Container instanser: överlappande tillvägagångssättet</span><span class="sxs-lookup"><span data-stu-id="d6e1b-119">Orchestration with Azure Container Instances: A layered approach</span></span>

<span data-ttu-id="d6e1b-120">Azure Behållarinstanser som aktiverar en överlappande tillvägagångssättet tooorchestration eftersom alla hello schemaläggning och hanteringsfunktioner krävs toorun en enskild behållare samtidigt orchestrator plattformar toomanage flera behållare uppgifter ovanpå den.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-120">Azure Container Instances enables a layered approach tooorchestration, providing all of hello scheduling and management capabilities required toorun a single container, while allowing orchestrator platforms toomanage multi-container tasks on top of it.</span></span>

<span data-ttu-id="d6e1b-121">Eftersom alla hello underliggande infrastruktur för Behållarinstanser som hanteras av Azure, behöver inte tooconcern med att hitta en lämplig värddator på vilken toorun en enskild behållare i en orchestrator-plattformen.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-121">Because all of hello underlying infrastructure for Container Instances is managed by Azure, an orchestrator platform does not need tooconcern itself with finding an appropriate host machine on which toorun a single container.</span></span> <span data-ttu-id="d6e1b-122">hello elasticitet för hello moln garanterar att en alltid är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-122">hello elasticity of hello cloud ensures that one is always available.</span></span> <span data-ttu-id="d6e1b-123">Hello orchestrator kan istället fokusera på hello uppgifter som förenklar hello utvecklingen av flera behållare arkitekturer, inklusive skalning och samordnade uppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-123">Instead, hello orchestrator can focus on hello tasks that simplify hello development of multi-container architectures, including scaling and coordinated upgrades.</span></span>



## <a name="potential-scenarios"></a><span data-ttu-id="d6e1b-124">Möjliga scenarier</span><span class="sxs-lookup"><span data-stu-id="d6e1b-124">Potential scenarios</span></span>

<span data-ttu-id="d6e1b-125">Orchestrator-integrering med Azure Container instanser är fortfarande framväxande, förutse att några olika miljöer kan du se ett mönster:</span><span class="sxs-lookup"><span data-stu-id="d6e1b-125">While orchestrator integration with Azure Container Instances is still nascent, we anticipate that a few different environments may emerge:</span></span>

### <a name="orchestration-of-container-instances-exclusively"></a><span data-ttu-id="d6e1b-126">Dirigering av Behållarinstanser exklusivt</span><span class="sxs-lookup"><span data-stu-id="d6e1b-126">Orchestration of Container Instances exclusively</span></span>

<span data-ttu-id="d6e1b-127">Eftersom de komma igång snabbt och debiterar av hello andra, en miljö baserat på enbart Behållarinstanser som Azure erbjuder hello snabbaste sättet tooget igång och toodeal med hög variabel arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-127">Because they start quickly and bill by hello second, an environment based exclusively on Azure Container Instances offers hello fastest way tooget started and toodeal with highly variable workloads.</span></span>

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a><span data-ttu-id="d6e1b-128">Kombinationen av Behållarinstanser och behållare i virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="d6e1b-128">Combination of Container Instances and Containers in Virtual Machines</span></span>

<span data-ttu-id="d6e1b-129">För tidskrävande blir stabil arbetsbelastningar, samordna behållare i ett kluster på dedikerade virtuella datorer vanligtvis billigare än att köra hello samma behållare med Behållarinstanser.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-129">For long-running, stable workloads, orchestrating containers in a cluster of dedicated virtual machines will typically be cheaper than running hello same containers with Container Instances.</span></span> <span data-ttu-id="d6e1b-130">Behållarinstanser erbjuder dock en bra lösning för snabbt expanderande och tilldelning av kontrakt din totala kapaciteten toodeal med oväntade eller tillfällig toppar i användning.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-130">However, Container Instances offer a great solution for quickly expanding and contracting your overall capacity toodeal with unexpected or short-lived spikes in usage.</span></span> <span data-ttu-id="d6e1b-131">I stället för att skala ut hello antalet virtuella datorer i klustret och sedan distribuera ytterligare behållare på de datorerna hello orchestrator kan bara schemalägga hello ytterligare behållare med Behållarinstanser och ta bort dem när de är ingen längre behövs.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-131">Rather than scaling out hello number of virtual machines in your cluster, then deploying additional containers onto those machines, hello orchestrator can simply schedule hello additional containers using Container Instances and delete them when they're no longer needed.</span></span>

## <a name="sample-implementation-azure-container-instances-connector-for-kubernetes"></a><span data-ttu-id="d6e1b-132">Exempel på implementering: Azure Container instanser Connector för Kubernetes</span><span class="sxs-lookup"><span data-stu-id="d6e1b-132">Sample implementation: Azure Container Instances Connector for Kubernetes</span></span>

<span data-ttu-id="d6e1b-133">toodemonstrate hur behållaren orchestration plattformar kan integrera med Azure Container instanser, vi har börjat skapa en [exempel connector för Kubernetes][aci-connector-k8s].</span><span class="sxs-lookup"><span data-stu-id="d6e1b-133">toodemonstrate how container orchestration platforms can integrate with Azure Container Instances, we have started building a [sample connector for Kubernetes][aci-connector-k8s].</span></span> 

<span data-ttu-id="d6e1b-134">Hej connector för Kubernetes efterliknar hello [kubelet] [ kubelet-doc] genom att registrera som en nod med obegränsad kapacitet och sändning av hello skapandet av [skida] [ pod-doc] som behållargrupper i Azure Container instanser.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-134">hello connector for Kubernetes mimics hello [kubelet][kubelet-doc] by registering as a node with unlimited capacity and dispatching hello creation of [pods][pod-doc] as container groups in Azure Container Instances.</span></span> 

<!-- ![ACI Connector for Kubernetes][aci-connector-k8s-gif] -->

<span data-ttu-id="d6e1b-135">Kopplingar för andra orchestrators kan byggas som på samma sätt är integrerad med plattformen primitiver toocombine hello power hello orchestrator API med hello hastighet och enkelhet för att hantera behållare i Azure Container instanser.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-135">Connectors for other orchestrators could be built that similarly integrated with platform primitives toocombine hello power of hello orchestrator API with hello speed and simplicity of managing containers in Azure Container Instances.</span></span>

> [!WARNING]
> <span data-ttu-id="d6e1b-136">Hej ACI connector för Kubernetes är *experiment* och ska inte användas i produktionen.</span><span class="sxs-lookup"><span data-stu-id="d6e1b-136">hello ACI connector for Kubernetes is *experimental* and should not be used in production.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6e1b-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d6e1b-137">Next steps</span></span>

<span data-ttu-id="d6e1b-138">Skapa din första behållare med Azure Container instanser med hello [snabbstartsguiden](container-instances-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="d6e1b-138">Create your first container with Azure Container Instances using hello [quick start guide](container-instances-quickstart.md).</span></span>

<!-- IMAGES -->
[aci-connector-k8s-gif]: ./media/container-instances-orchestrator-relationship/aci-connector-k8s.gif

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/azure/aci-connector-k8s
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/