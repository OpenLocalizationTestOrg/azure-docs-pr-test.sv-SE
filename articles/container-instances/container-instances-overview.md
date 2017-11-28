---
title: "aaaAzure behållaren instanser översikt | Azure-dokument"
description: "Förstå Azure Container Instances"
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
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c0662ede1260b15d9841bfc2c3c4cec4c30338d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-instances"></a><span data-ttu-id="a0227-103">Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="a0227-103">Azure Container Instances</span></span>

<span data-ttu-id="a0227-104">Behållare blir snabbt hello önskad sätt toopackage, distribuera och hantera molnprogram.</span><span class="sxs-lookup"><span data-stu-id="a0227-104">Containers are quickly becoming hello preferred way toopackage, deploy, and manage cloud applications.</span></span> <span data-ttu-id="a0227-105">Behållarinstanser som Azure erbjuder hello snabbaste och enklaste sättet toorun en behållare i Azure, utan att behöva tooprovision alla virtuella datorer och utan att behöva tooadopt en högre nivå tjänst.</span><span class="sxs-lookup"><span data-stu-id="a0227-105">Azure Container Instances offers hello fastest and simplest way toorun a container in Azure, without having tooprovision any virtual machines and without having tooadopt a higher-level service.</span></span> 

<span data-ttu-id="a0227-106">Azure Container Instances är en bra lösning för alla scenarier som kan fungera i isolerade behållare, däribland enkla program, automatisering av uppgifter och att skapa jobb.</span><span class="sxs-lookup"><span data-stu-id="a0227-106">Azure Container Instances is a great solution for any scenario that can operate in isolated containers, including simple applications, task automation, and build jobs.</span></span> <span data-ttu-id="a0227-107">För scenarier där du måste ha fullständig behållaren orchestration, inklusive tjänstidentifiering över flera behållare, automatisk skalning och samordnade programuppgraderingar rekommenderar vi hello [Azure Container Service](https://docs.microsoft.com/azure/container-service/).</span><span class="sxs-lookup"><span data-stu-id="a0227-107">For scenarios where you need full container orchestration, including service discovery across multiple containers, automatic scaling, and coordinated application upgrades, we recommend hello [Azure Container Service](https://docs.microsoft.com/azure/container-service/).</span></span>

## <a name="fast-startup-times"></a><span data-ttu-id="a0227-108">Snabba starttider</span><span class="sxs-lookup"><span data-stu-id="a0227-108">Fast startup times</span></span>

<span data-ttu-id="a0227-109">Med behållare får du betydande startfördelar jämfört med virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a0227-109">Containers offer significant startup benefits over virtual machines.</span></span> <span data-ttu-id="a0227-110">Du kan starta en behållare i Azure i sekunder utan hello måste tooprovision och hantera virtuella datorer med Azure Container instanser.</span><span class="sxs-lookup"><span data-stu-id="a0227-110">With Azure Container Instances, you can start a container in Azure in seconds without hello need tooprovision and manage VMs.</span></span>

## <a name="hypervisor-level-security"></a><span data-ttu-id="a0227-111">Säkerhet på hypervisornivå</span><span class="sxs-lookup"><span data-stu-id="a0227-111">Hypervisor-level security</span></span>

<span data-ttu-id="a0227-112">Tidigare har behållare erbjudit isolering av programberoenden och resursstyrning men har inte ansetts vara tillräckligt strikta för fientlig användning med flera innehavare.</span><span class="sxs-lookup"><span data-stu-id="a0227-112">Historically, containers have offered application dependency isolation and resource governance but have not been considered sufficiently hardened for hostile multi-tenant usage.</span></span> <span data-ttu-id="a0227-113">Med Azure Container Instances är ditt program lika isolerat i en behållare som i en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a0227-113">With Azure Container Instances, your application is as isolated in a container as it would be in a VM.</span></span>

## <a name="custom-sizes"></a><span data-ttu-id="a0227-114">Anpassade storlekar</span><span class="sxs-lookup"><span data-stu-id="a0227-114">Custom sizes</span></span>

<span data-ttu-id="a0227-115">Behållare är vanligtvis optimerade toorun bara ett enda program, men hello exakta behov av dessa program kan variera avsevärt.</span><span class="sxs-lookup"><span data-stu-id="a0227-115">Containers are typically optimized toorun just a single application, but hello exact needs of those applications can differ greatly.</span></span> <span data-ttu-id="a0227-116">Med Azure Container Instances kan du begära exakt det du behöver när det gäller processorkärnor och minne.</span><span class="sxs-lookup"><span data-stu-id="a0227-116">With Azure Container Instances, you can request exactly what you need in terms of CPU cores and memory.</span></span> <span data-ttu-id="a0227-117">Du betalar utifrån vad du begär fakturerats av hello andra, så du kan finjustera optimera dina utgifter baserat på dina behov.</span><span class="sxs-lookup"><span data-stu-id="a0227-117">You pay based on what you request, billed by hello second, so you can finely optimize your spending based on your needs.</span></span>

## <a name="public-ip-connectivity"></a><span data-ttu-id="a0227-118">Offentlig IP-anslutning</span><span class="sxs-lookup"><span data-stu-id="a0227-118">Public IP connectivity</span></span>

<span data-ttu-id="a0227-119">Med Azure Container instanser, kan du exponera behållarna direkt toohello internet med en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a0227-119">With Azure Container Instances, you can expose your containers directly toohello internet with a public IP address.</span></span> <span data-ttu-id="a0227-120">I framtida hello, kommer vi Expandera våra nätverk funktioner tooinclude integrering med virtuella nätverk, belastningsutjämnare och andra delar av hello Azure nätverksinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="a0227-120">In hello future, we will expand our networking capabilities tooinclude integration with virtual networks, load balancers, and other core parts of hello Azure networking infrastructure.</span></span>

## <a name="persistent-storage"></a><span data-ttu-id="a0227-121">Beständig lagring</span><span class="sxs-lookup"><span data-stu-id="a0227-121">Persistent storage</span></span>

<span data-ttu-id="a0227-122">tooretrieve och beständiga tillstånd med Azure Container instanser, vi erbjuder direkt montering av Azure filer resurser.</span><span class="sxs-lookup"><span data-stu-id="a0227-122">tooretrieve and persist state with Azure Container Instances, we offer direct mounting of Azure files shares.</span></span>

## <a name="linux-and-windows-containers"></a><span data-ttu-id="a0227-123">Linux- och Windows-behållare</span><span class="sxs-lookup"><span data-stu-id="a0227-123">Linux and Windows containers</span></span>

<span data-ttu-id="a0227-124">Du kan schemalägga både Windows och Linux-behållare med hello samma API med Azure Container instanser.</span><span class="sxs-lookup"><span data-stu-id="a0227-124">With Azure Container Instances, you can schedule both Windows and Linux containers with hello same API.</span></span> <span data-ttu-id="a0227-125">Bara ange hello OS bastypen och allt annat är identiska.</span><span class="sxs-lookup"><span data-stu-id="a0227-125">Simply indicate hello base OS type and everything else is identical.</span></span>

## <a name="co-scheduled-groups"></a><span data-ttu-id="a0227-126">Samordna schemalagda grupper</span><span class="sxs-lookup"><span data-stu-id="a0227-126">Co-scheduled groups</span></span>

<span data-ttu-id="a0227-127">Azure Container Instances stöder schemaläggning av grupper med flera behållare som delar en värddator, lokalt nätverk, lagring och livscykel.</span><span class="sxs-lookup"><span data-stu-id="a0227-127">Azure Container Instances supports scheduling of multi-container groups that share a host machine, local network, storage, and lifecycle.</span></span> <span data-ttu-id="a0227-128">Detta gör att du toocombine huvudsakliga programmet med andra i en stödjande roll, t.ex loggning.</span><span class="sxs-lookup"><span data-stu-id="a0227-128">This enables you toocombine your main application with others acting in a supporting role, such as logging.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0227-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a0227-129">Next steps</span></span>

<span data-ttu-id="a0227-130">Försök att distribuera en behållare tooAzure med ett enda kommando med hjälp av vår [Snabbstartsguide](container-instances-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="a0227-130">Try deploying a container tooAzure with a single command using our [quickstart guide](container-instances-quickstart.md).</span></span>
