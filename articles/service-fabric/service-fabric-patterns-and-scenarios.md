---
title: "aaaAzure Service Fabric mönster och scenarier | Microsoft Docs"
description: "Lär dig bästa praxis och beprövade återanvändbara mönster toodesign utveckla och hantera din mikrotjänster på Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: d5aa75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2017
ms.author: ryanwi
ms.openlocfilehash: 3811420eb53d9a49e490dd2e2e5319d8dea5629c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-patterns-and-scenarios"></a><span data-ttu-id="b5d02-103">Service Fabric-mönster och scenarier</span><span class="sxs-lookup"><span data-stu-id="b5d02-103">Service Fabric patterns and scenarios</span></span>
<span data-ttu-id="b5d02-104">Om du tittar på bygga storskaliga mikrotjänster med hjälp av Azure Service Fabric, lär du dig från hello-experter som designas och skapas den här plattformen som en tjänst (PaaS).</span><span class="sxs-lookup"><span data-stu-id="b5d02-104">If you’re looking at building large-scale microservices using Azure Service Fabric, learn from hello experts who designed and built this platform as a service (PaaS).</span></span> <span data-ttu-id="b5d02-105">Kom igång med rätt arkitektur och lär dig hur toooptimize resurser för ditt program.</span><span class="sxs-lookup"><span data-stu-id="b5d02-105">Get started with proper architecture, and then learn how toooptimize resources for your application.</span></span> <span data-ttu-id="b5d02-106">Hej [Service Fabric Patterns and Practices](https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344) kursen svar hello frågor oftast av verkliga kunder om Service Fabric-scenarier och moduler.</span><span class="sxs-lookup"><span data-stu-id="b5d02-106">hello [Service Fabric Patterns and Practices](https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344) course answers hello questions most often asked by real-world customers about Service Fabric scenarios and application areas.</span></span>
 
<span data-ttu-id="b5d02-107">Ta reda på hur toodesign, utveckla och hantera din mikrotjänster på Service Fabric med bästa praxis och beprövade, återanvändbara mönster.</span><span class="sxs-lookup"><span data-stu-id="b5d02-107">Find out how toodesign, develop, and operate your microservices on Service Fabric using best practices and proven, reusable patterns.</span></span> <span data-ttu-id="b5d02-108">Få en översikt över Service Fabric och sedan sätta igång djupare i avsnitten som innehåller klustret optimering och säkerhet, migrera äldre appar IoT i skala, värd för spel motorer och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="b5d02-108">Get an overview of Service Fabric and then dive deep into topics that cover cluster optimization and security, migrating legacy apps, IoT at scale, hosting game engines, and more.</span></span> <span data-ttu-id="b5d02-109">Titta på kontinuerlig leverans för olika arbetsbelastningar och även få hello information om Linux-support och behållare.</span><span class="sxs-lookup"><span data-stu-id="b5d02-109">Look at continuous delivery for various workloads, and even get hello details on Linux support and containers.</span></span> 

## <a name="introduction"></a><span data-ttu-id="b5d02-110">Introduktion</span><span class="sxs-lookup"><span data-stu-id="b5d02-110">Introduction</span></span>
<span data-ttu-id="b5d02-111">Utforska metodtips och lär dig mer om hur du väljer plattform som en tjänst (PaaS) över infrastruktur som en tjänst (IaaS).</span><span class="sxs-lookup"><span data-stu-id="b5d02-111">Explore best practices, and learn about choosing platform as a service (PaaS) over infrastructure as a service (IaaS).</span></span> <span data-ttu-id="b5d02-112">Hämta hello information på följande program beprövad design principer.</span><span class="sxs-lookup"><span data-stu-id="b5d02-112">Get hello details on following proven application design principles.</span></span>

<table><tr><th><span data-ttu-id="b5d02-113">Video</span><span class="sxs-lookup"><span data-stu-id="b5d02-113">Video</span></span></th><th><span data-ttu-id="b5d02-114">PowerPoint däcket</span><span class="sxs-lookup"><span data-stu-id="b5d02-114">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=N2KwbbSGD_6405167344">
<img src="./media/service-fabric-patterns-and-scenarios/intro.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="b5d02-115"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344">Introduktion tooService Fabric</a></span><span class="sxs-lookup"><span data-stu-id="b5d02-115"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344">Introduction tooService Fabric</a></span></span></td></tr>
</table>

## <a name="cluster-planning-and-management"></a><span data-ttu-id="b5d02-116">Planera kluster och hantering</span><span class="sxs-lookup"><span data-stu-id="b5d02-116">Cluster planning and management</span></span>
<span data-ttu-id="b5d02-117">Lär dig mer om kapacitetsplanering klustret optimering och klustret säkerhet i den här titt på Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b5d02-117">Learn about capacity planning, cluster optimization, and cluster security, in this look at Azure Service Fabric.</span></span>

<table><tr><th><span data-ttu-id="b5d02-118">Video</span><span class="sxs-lookup"><span data-stu-id="b5d02-118">Video</span></span></th><th><span data-ttu-id="b5d02-119">PowerPoint däcket</span><span class="sxs-lookup"><span data-stu-id="b5d02-119">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=cyDYZcSGD_2805167344">
<img src="./media/service-fabric-patterns-and-scenarios/cluster.png" WIDTH="360" HEIGHT="244">
</a></td><td> <span data-ttu-id="b5d02-120"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=E5B3nJSGD_805167344">Planera kluster och hantering</a></span><span class="sxs-lookup"><span data-stu-id="b5d02-120"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=E5B3nJSGD_805167344">Cluster Planning and Management</a></span></span></td></tr>
</table>

## <a name="hyper-scale-web"></a><span data-ttu-id="b5d02-121">Hyper-skala webbprogram</span><span class="sxs-lookup"><span data-stu-id="b5d02-121">Hyper-scale web</span></span>
<span data-ttu-id="b5d02-122">Granska begreppen runt storskaliga Internet, till exempel tillgänglighet och tillförlitlighet, storskaliga och tillståndshantering av.</span><span class="sxs-lookup"><span data-stu-id="b5d02-122">Review concepts around hyper-scale web, including availability and reliability, hyper-scale, and state management.</span></span>

<table><tr><th><span data-ttu-id="b5d02-123">Video</span><span class="sxs-lookup"><span data-stu-id="b5d02-123">Video</span></span></th><th><span data-ttu-id="b5d02-124">PowerPoint däcket</span><span class="sxs-lookup"><span data-stu-id="b5d02-124">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=NgldAdSGD_405167344">
<img src="./media/service-fabric-patterns-and-scenarios/hyperscaleweb.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="b5d02-125"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=CPMLBLSGD_7705167344">Hyper-skala webbprogram</a></span><span class="sxs-lookup"><span data-stu-id="b5d02-125"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=CPMLBLSGD_7705167344">Hyper-scale web</a></span></span></td></tr>
</table>

## <a name="iot"></a><span data-ttu-id="b5d02-126">IoT</span><span class="sxs-lookup"><span data-stu-id="b5d02-126">IoT</span></span>
<span data-ttu-id="b5d02-127">Utforska hello Sakernas Internet (IoT) i Azure Service Fabric, inklusive hello Azure IoT pipeline, multitenans och IoT i skala hello kontext.</span><span class="sxs-lookup"><span data-stu-id="b5d02-127">Explore hello Internet of Things (IoT) in hello context of Azure Service Fabric, including hello Azure IoT pipeline, multi-tenancy, and IoT at scale.</span></span>

<table><tr><th><span data-ttu-id="b5d02-128">Video</span><span class="sxs-lookup"><span data-stu-id="b5d02-128">Video</span></span></th><th><span data-ttu-id="b5d02-129">PowerPoint däcket</span><span class="sxs-lookup"><span data-stu-id="b5d02-129">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=naFUVeSGD_1505167344">
<img src="./media/service-fabric-patterns-and-scenarios/iot.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="b5d02-130"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">IoT</a></span><span class="sxs-lookup"><span data-stu-id="b5d02-130"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">IoT</a></span></span></td></tr>
</table>

## <a name="gaming"></a><span data-ttu-id="b5d02-131">Spel</span><span class="sxs-lookup"><span data-stu-id="b5d02-131">Gaming</span></span>
<span data-ttu-id="b5d02-132">Titta på bygger spel, interaktiva spel och värd befintliga spel motorer.</span><span class="sxs-lookup"><span data-stu-id="b5d02-132">Look at turn-based games, interactive games, and hosting existing game engines.</span></span>

<table><tr><th><span data-ttu-id="b5d02-133">Video</span><span class="sxs-lookup"><span data-stu-id="b5d02-133">Video</span></span></th><th><span data-ttu-id="b5d02-134">PowerPoint däcket</span><span class="sxs-lookup"><span data-stu-id="b5d02-134">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=6wECzeSGD_3805167344">
<img src="./media/service-fabric-patterns-and-scenarios/gaming.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="b5d02-135"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">Spel</a></span><span class="sxs-lookup"><span data-stu-id="b5d02-135"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">Gaming</a></span></span></td></tr>
</table>

## <a name="continuous-delivery"></a><span data-ttu-id="b5d02-136">Kontinuerlig leverans</span><span class="sxs-lookup"><span data-stu-id="b5d02-136">Continuous delivery</span></span>
<span data-ttu-id="b5d02-137">Utforska begrepp, inklusive kontinuerlig integration/kontinuerlig leverans med Visual Studio Team Services, build/paket/publicera arbetsflödet, flera miljökonfiguration och tjänsten paketresursen.</span><span class="sxs-lookup"><span data-stu-id="b5d02-137">Explore concepts, including continuous integration/continuous delivery with Visual Studio Team Services, build/package/publish workflow, multi-environment setup, and service package/share.</span></span>

<table><tr><th><span data-ttu-id="b5d02-138">Video</span><span class="sxs-lookup"><span data-stu-id="b5d02-138">Video</span></span></th><th><span data-ttu-id="b5d02-139">PowerPoint däcket</span><span class="sxs-lookup"><span data-stu-id="b5d02-139">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=78h5ofSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/cd.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="b5d02-140"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=VlENvOSGD_105167344">Kontinuerlig leverans</a></span><span class="sxs-lookup"><span data-stu-id="b5d02-140"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=VlENvOSGD_105167344">Continuous delivery</a></span></span></td></tr>
</table>

## <a name="migration"></a><span data-ttu-id="b5d02-141">Migrering</span><span class="sxs-lookup"><span data-stu-id="b5d02-141">Migration</span></span>
<span data-ttu-id="b5d02-142">Lär dig mer om hur du migrerar från en molnbaserad tjänst bör dessutom toomigration för äldre appar.</span><span class="sxs-lookup"><span data-stu-id="b5d02-142">Learn about migrating from a cloud service, in addition toomigration of legacy apps.</span></span>

<table><tr><th><span data-ttu-id="b5d02-143">Video</span><span class="sxs-lookup"><span data-stu-id="b5d02-143">Video</span></span></th><th><span data-ttu-id="b5d02-144">PowerPoint däcket</span><span class="sxs-lookup"><span data-stu-id="b5d02-144">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=hd1cMgSGD_5605167344">
<img src="./media/service-fabric-patterns-and-scenarios/migration.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="b5d02-145"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=GQAq4QSGD_8305167344">Migrering</a></span><span class="sxs-lookup"><span data-stu-id="b5d02-145"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=GQAq4QSGD_8305167344">Migration</a></span></span></td></tr>
</table>

## <a name="containers-and-linux-support"></a><span data-ttu-id="b5d02-146">Behållare och Linux-support</span><span class="sxs-lookup"><span data-stu-id="b5d02-146">Containers and Linux support</span></span>
<span data-ttu-id="b5d02-147">Hämta hello för toohello fråga ”varför behållare”?</span><span class="sxs-lookup"><span data-stu-id="b5d02-147">Get hello answer toohello question, "Why containers?"</span></span> <span data-ttu-id="b5d02-148">Läs mer om hello förhandsgranskning för Windows-behållare, har stöd för Linux- och Linux behållare orchestration.</span><span class="sxs-lookup"><span data-stu-id="b5d02-148">Learn about hello preview for Windows containers, Linux supports, and Linux containers orchestration.</span></span> <span data-ttu-id="b5d02-149">Dessutom kan du ta reda på hur toomigrate .NET Core appar tooLinux.</span><span class="sxs-lookup"><span data-stu-id="b5d02-149">Plus, find out how toomigrate .NET Core apps tooLinux.</span></span>

<table><tr><th><span data-ttu-id="b5d02-150">Video</span><span class="sxs-lookup"><span data-stu-id="b5d02-150">Video</span></span></th><th><span data-ttu-id="b5d02-151">PowerPoint däcket</span><span class="sxs-lookup"><span data-stu-id="b5d02-151">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=V1ERJhSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/containers.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="b5d02-152"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mlYsZRSGD_2105167344">Behållare och Linux-support</a></span><span class="sxs-lookup"><span data-stu-id="b5d02-152"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mlYsZRSGD_2105167344">Containers and Linux support</a></span></span></td></tr>
</table>

## <a name="next-steps"></a><span data-ttu-id="b5d02-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5d02-153">Next steps</span></span>
<span data-ttu-id="b5d02-154">Nu när du har lärt dig om Service Fabric-mönster och scenarier, Läs mer om hur för[skapa och hantera kluster](service-fabric-deploy-anywhere.md), [migrera molntjänster appar tooService Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md), [ställa in kontinuerlig leverans](service-fabric-set-up-continuous-integration.md), och [distribuera behållare](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b5d02-154">Now that you've learned about Service Fabric patterns and scenarios, read more about how too[create and manage clusters](service-fabric-deploy-anywhere.md), [migrate Cloud Services apps tooService Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md), [set up continuous delivery](service-fabric-set-up-continuous-integration.md), and [deploy containers](service-fabric-containers-overview.md).</span></span>
