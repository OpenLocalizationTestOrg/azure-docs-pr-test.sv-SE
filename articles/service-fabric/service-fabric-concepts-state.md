---
title: "aaaDefinine och hantera tillstånd i Azure mikrotjänster | Microsoft Docs"
description: "Hur toodefine och hantera i Service Fabric-tjänstens tillstånd"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: f5e618a5-3ea3-4404-94af-122278f91652
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 4a24696da71753d0f343a86df3556b5b7c964834
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-state"></a><span data-ttu-id="3b1fc-103">Tillstånd för tjänsten</span><span class="sxs-lookup"><span data-stu-id="3b1fc-103">Service state</span></span>
<span data-ttu-id="3b1fc-104">**Tjänsten tillstånd** refererar toohello i minnet eller på att en tjänst kräver toofunction diskdata.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-104">**Service state** refers toohello in-memory or on disk data that a service requires toofunction.</span></span> <span data-ttu-id="3b1fc-105">Den omfattar, till exempel hello datastrukturer och medlemsvariabler hello tjänsten läser och skriver toodo arbete.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-105">It includes, for example, hello data structures and member variables that hello service reads and writes toodo work.</span></span> <span data-ttu-id="3b1fc-106">Beroende på hur hello-tjänsten är konstruerad, kan den också innehålla filer eller andra resurser som är lagrade på disken.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-106">Depending on how hello service is architected, it could also include files or other resources that are stored on disk.</span></span> <span data-ttu-id="3b1fc-107">Till exempel hello filer en-databas skulle använda toostore data och transaktionsloggarna.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-107">For example, hello files a database would use toostore data and transaction logs.</span></span>

<span data-ttu-id="3b1fc-108">Nu ska vi titta Kalkylatorn som en exempel-tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-108">As an example service, let's consider a calculator.</span></span> <span data-ttu-id="3b1fc-109">En enkel kalkylator tjänst tar två tal och returnerar sina summan.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-109">A basic calculator service takes two numbers and returns their sum.</span></span> <span data-ttu-id="3b1fc-110">Utför den här beräkningen innebär att inga Medlemsvariabler eller annan information.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-110">Performing this calculation involves no member variables or other information.</span></span>

<span data-ttu-id="3b1fc-111">Tänk dig nu hello samma Kalkylatorn, men det har beräknats med ytterligare en metod för att lagra och returnera hello sista summan.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-111">Now consider hello same calculator, but with an additional method for storing and returning hello last sum it has computed.</span></span> <span data-ttu-id="3b1fc-112">Den här tjänsten är nu tillståndskänsliga.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-112">This service is now stateful.</span></span> <span data-ttu-id="3b1fc-113">Stateful innebär att den innehåller vissa tillstånd som skriver den toowhen den beräknar en ny summa och läser från när du begär tooreturn hello senaste beräknade summan.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-113">Stateful means it contains some state that it writes toowhen it computes a new sum and reads from when you ask it tooreturn hello last computed sum.</span></span>

<span data-ttu-id="3b1fc-114">I Azure Service Fabric kallas hello första tjänsten tillståndslösa tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-114">In Azure Service Fabric, hello first service is called a stateless service.</span></span> <span data-ttu-id="3b1fc-115">hello andra tjänsten kallas en tillståndskänslig service.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-115">hello second service is called a stateful service.</span></span>

## <a name="storing-service-state"></a><span data-ttu-id="3b1fc-116">Spara tjänstens tillstånd</span><span class="sxs-lookup"><span data-stu-id="3b1fc-116">Storing service state</span></span>
<span data-ttu-id="3b1fc-117">Tillstånd kan vara antingen externalized eller samordnad med hello-kod som manipulering hello tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-117">State can be either externalized or co-located with hello code that is manipulating hello state.</span></span> <span data-ttu-id="3b1fc-118">Externalization i tillståndet normalt görs med hjälp av en extern databas eller andra data lagras att hello körs på olika datorer hello nätverket eller utanför processen på samma dator.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-118">Externalization of state is typically done by using an external database or other data store that runs on different machines over hello network or out of process on hello same machine.</span></span> <span data-ttu-id="3b1fc-119">I vårt exempel Kalkylatorn kan hello datalagret vara en SQL-databas eller en instans av Azure Table Store.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-119">In our calculator example, hello data store could be a SQL database or instance of Azure Table Store.</span></span> <span data-ttu-id="3b1fc-120">Varje begäran toocompute hello summan utför en uppdatering på dessa data och begär toohello tjänsten tooreturn hello värde resulterar i hello aktuellt värde som hämtats från hello store.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-120">Every request toocompute hello sum performs an update on this data, and requests toohello service tooreturn hello value result in hello current value being fetched from hello store.</span></span> 

<span data-ttu-id="3b1fc-121">Tillståndet kan också vara samordnad med hello-kod som hanterar hello tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-121">State can also be co-located with hello code that manipulates hello state.</span></span> <span data-ttu-id="3b1fc-122">Tillståndskänsliga tjänster i Service Fabric skapas vanligtvis med hjälp av den här modellen.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-122">Stateful services in Service Fabric are typically built using this model.</span></span> <span data-ttu-id="3b1fc-123">Service Fabric ger hello infrastruktur tooensure att det här tillståndet har hög tillgänglighet, konsekvent och varaktig och att hello services bygger det här sättet kan enkelt skala.</span><span class="sxs-lookup"><span data-stu-id="3b1fc-123">Service Fabric provides hello infrastructure tooensure that this state is highly available, consistent, and durable, and that hello services built this way can easily scale.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b1fc-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3b1fc-124">Next steps</span></span>
<span data-ttu-id="3b1fc-125">Mer information om Service Fabric-begrepp finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="3b1fc-125">For more information on Service Fabric concepts, see hello following articles:</span></span>

* [<span data-ttu-id="3b1fc-126">Tillgänglighet för Service Fabric-tjänster</span><span class="sxs-lookup"><span data-stu-id="3b1fc-126">Availability of Service Fabric services</span></span>](service-fabric-availability-services.md)
* [<span data-ttu-id="3b1fc-127">Skalbarheten för Service Fabric-tjänster</span><span class="sxs-lookup"><span data-stu-id="3b1fc-127">Scalability of Service Fabric services</span></span>](service-fabric-concepts-scalability.md)
* [<span data-ttu-id="3b1fc-128">Partitionering Service Fabric-tjänster</span><span class="sxs-lookup"><span data-stu-id="3b1fc-128">Partitioning Service Fabric services</span></span>](service-fabric-concepts-partitioning.md)
* [<span data-ttu-id="3b1fc-129">Service Fabric Reliable Services</span><span class="sxs-lookup"><span data-stu-id="3b1fc-129">Service Fabric Reliable Services</span></span>](service-fabric-reliable-services-introduction.md)
