---
title: "Partitionering Service Fabric-tjänster | Microsoft Docs"
description: "Beskriver hur du partitionera tillståndskänslig Service Fabric-tjänster. Partitioner möjliggör lagring av data på lokala datorer så att data och beräkning kan skalas tillsammans."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 3b7248c8-ea92-4964-85e7-6f1291b5cc7b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: msfussell
ms.openlocfilehash: 3c1e80305cb65f41a6981b99f69e8b87f89599ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="partition-service-fabric-reliable-services"></a><span data-ttu-id="606a1-104">Partitionen Service Fabric reliable services</span><span class="sxs-lookup"><span data-stu-id="606a1-104">Partition Service Fabric reliable services</span></span>
<span data-ttu-id="606a1-105">Den här artikeln innehåller en introduktion till de grundläggande principerna för partitionering tillförlitliga Azure Service Fabric-tjänster.</span><span class="sxs-lookup"><span data-stu-id="606a1-105">This article provides an introduction to the basic concepts of partitioning Azure Service Fabric reliable services.</span></span> <span data-ttu-id="606a1-106">Källkoden som används i artikeln finns även på [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span><span class="sxs-lookup"><span data-stu-id="606a1-106">The source code used in the article is also available on [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span></span>

## <a name="partitioning"></a><span data-ttu-id="606a1-107">Partitionering</span><span class="sxs-lookup"><span data-stu-id="606a1-107">Partitioning</span></span>
<span data-ttu-id="606a1-108">Partitionering är inte unikt för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="606a1-108">Partitioning is not unique to Service Fabric.</span></span> <span data-ttu-id="606a1-109">Det är faktiskt en core mönster för att skapa skalbara tjänster.</span><span class="sxs-lookup"><span data-stu-id="606a1-109">In fact, it is a core pattern of building scalable services.</span></span> <span data-ttu-id="606a1-110">I en bredare mening vi tror om partitionering som ett koncept för att dela tillstånd (data) och beräkna till mindre tillgänglig enheter för att förbättra skalbarhet och prestanda.</span><span class="sxs-lookup"><span data-stu-id="606a1-110">In a broader sense, we can think about partitioning as a concept of dividing state (data) and compute into smaller accessible units to improve scalability and performance.</span></span> <span data-ttu-id="606a1-111">Ett välkänt formulär partitionering är [Datapartitionering][wikipartition], även kallad horisontell partitionering.</span><span class="sxs-lookup"><span data-stu-id="606a1-111">A well-known form of partitioning is [data partitioning][wikipartition], also known as sharding.</span></span>

### <a name="partition-service-fabric-stateless-services"></a><span data-ttu-id="606a1-112">Tillståndslösa tjänster för Service Fabric för partition</span><span class="sxs-lookup"><span data-stu-id="606a1-112">Partition Service Fabric stateless services</span></span>
<span data-ttu-id="606a1-113">Du kan se om en partition är en logisk enhet som innehåller en eller flera instanser av en tjänst för tillståndslösa tjänster.</span><span class="sxs-lookup"><span data-stu-id="606a1-113">For stateless services, you can think about a partition being a logical unit that contains one or more instances of a service.</span></span> <span data-ttu-id="606a1-114">Bild 1 visar tillståndslösa tjänsten med fem instanser distribuerade i ett kluster med en partition.</span><span class="sxs-lookup"><span data-stu-id="606a1-114">Figure 1 shows a stateless service with five instances distributed across a cluster using one partition.</span></span>

![Tillståndslösa tjänsten](./media/service-fabric-concepts-partitioning/statelessinstances.png)

<span data-ttu-id="606a1-116">Det finns två typer av tillståndslösa tjänstelösningar verkligen.</span><span class="sxs-lookup"><span data-stu-id="606a1-116">There are really two types of stateless service solutions.</span></span> <span data-ttu-id="606a1-117">Den första är en tjänst som kvarstår dess tillstånd externt, till exempel i en Azure SQL database (till exempel en webbplats som lagrar sessionsinformation och data).</span><span class="sxs-lookup"><span data-stu-id="606a1-117">The first one is a service that persists its state externally, for example in an Azure SQL database (like a website that stores the session information and data).</span></span> <span data-ttu-id="606a1-118">Den andra är endast beräkning-tjänster (till exempel Kalkylatorn eller image thumbnailing) som inte hanterar några beständiga tillstånd.</span><span class="sxs-lookup"><span data-stu-id="606a1-118">The second one is computation-only services (like a calculator or image thumbnailing) that do not manage any persistent state.</span></span>

<span data-ttu-id="606a1-119">I antingen fallet partitionering tillståndslösa tjänsten är ett mycket sällsynt scenario--skalbarhet och tillgänglighet uppnås normalt genom att lägga till flera instanser.</span><span class="sxs-lookup"><span data-stu-id="606a1-119">In either case, partitioning a stateless service is a very rare scenario--scalability and availability are normally achieved by adding more instances.</span></span> <span data-ttu-id="606a1-120">Den enda gång som du vill överväga flera partitioner för tillståndslösa tjänstinstanser är när du behöver för att uppfylla särskilda routning begäran.</span><span class="sxs-lookup"><span data-stu-id="606a1-120">The only time you want to consider multiple partitions for stateless service instances is when you need to meet special routing requests.</span></span>

<span data-ttu-id="606a1-121">Exempelvis bör du fall där användare med ID: N i ett visst intervall endast hanteras av en viss tjänst-instans.</span><span class="sxs-lookup"><span data-stu-id="606a1-121">As an example, consider a case where users with IDs in a certain range should only be served by a particular service instance.</span></span> <span data-ttu-id="606a1-122">Ett annat exempel på när du gick partitionera en tillståndslös tjänst är när du har en verkligen partitionerade serverdel (t.ex. ett delat SQL database) och du vill styra vilken tjänstinstans ska skriva till databasen Fragmentera-- eller utföra annat arbete förberedelse inom den tillståndslösa tjänsten som kräver samma partitionering information som används i serverdelen.</span><span class="sxs-lookup"><span data-stu-id="606a1-122">Another example of when you could partition a stateless service is when you have a truly partitioned backend (e.g. a sharded SQL database) and you want to control which service instance should write to the database shard--or perform other preparation work within the stateless service that requires the same partitioning information as is used in the backend.</span></span> <span data-ttu-id="606a1-123">Dessa typer av scenarier kan också lösas på olika sätt och kräver inte nödvändigtvis service partitionering.</span><span class="sxs-lookup"><span data-stu-id="606a1-123">Those types of scenarios can also be solved in different ways and do not necessarily require service partitioning.</span></span>

<span data-ttu-id="606a1-124">Resten av den här genomgången fokuserar på tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="606a1-124">The remainder of this walkthrough focuses on stateful services.</span></span>

### <a name="partition-service-fabric-stateful-services"></a><span data-ttu-id="606a1-125">Tillståndskänsliga tjänster för Service Fabric för partition</span><span class="sxs-lookup"><span data-stu-id="606a1-125">Partition Service Fabric stateful services</span></span>
<span data-ttu-id="606a1-126">Service Fabric gör det enkelt att utveckla skalbara tillståndskänsliga tjänster genom att erbjuda förstklassig kan partition tillstånd (data).</span><span class="sxs-lookup"><span data-stu-id="606a1-126">Service Fabric makes it easy to develop scalable stateful services by offering a first-class way to partition state (data).</span></span> <span data-ttu-id="606a1-127">Begreppsmässigt, du kan se om en partition av en tillståndskänslig service som en skalningsenhet som är mycket pålitlig via [repliker](service-fabric-availability-services.md) som distribueras och balanserat över noder i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="606a1-127">Conceptually, you can think about a partition of a stateful service as a scale unit that is highly reliable through [replicas](service-fabric-availability-services.md) that are distributed and balanced across the nodes in a cluster.</span></span>

<span data-ttu-id="606a1-128">Partitionering i samband med Service Fabric tillståndskänsliga tjänster refererar till process för att fastställa att en viss tjänst partition är ansvarig för en del av den fullständiga statusen för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="606a1-128">Partitioning in the context of Service Fabric stateful services refers to the process of determining that a particular service partition is responsible for a portion of the complete state of the service.</span></span> <span data-ttu-id="606a1-129">(Som tidigare nämnts, en partition är en uppsättning [repliker](service-fabric-availability-services.md)).</span><span class="sxs-lookup"><span data-stu-id="606a1-129">(As mentioned before, a partition is a set of [replicas](service-fabric-availability-services.md)).</span></span> <span data-ttu-id="606a1-130">En fantastiska med Service Fabric är att den placerar partitionerna på olika noder.</span><span class="sxs-lookup"><span data-stu-id="606a1-130">A great thing about Service Fabric is that it places the partitions on different nodes.</span></span> <span data-ttu-id="606a1-131">Detta innebär att de ska växa till en nod resursgräns.</span><span class="sxs-lookup"><span data-stu-id="606a1-131">This allows them to grow to a node's resource limit.</span></span> <span data-ttu-id="606a1-132">När data behöver växa, partitioner växer och Service Fabric balanserar partitioner mellan noder.</span><span class="sxs-lookup"><span data-stu-id="606a1-132">As the data needs grow, partitions grow, and Service Fabric rebalances partitions across nodes.</span></span> <span data-ttu-id="606a1-133">Detta säkerställer fortsatt effektiv användning av maskinvaruresurser.</span><span class="sxs-lookup"><span data-stu-id="606a1-133">This ensures the continued efficient use of hardware resources.</span></span>

<span data-ttu-id="606a1-134">För att ge dig ett exempel, anta att du börjar med ett kluster med 5 och en tjänst som är konfigurerad med 10 partitioner och ett mål för tre repliker.</span><span class="sxs-lookup"><span data-stu-id="606a1-134">To give you an example, say you start with a 5-node cluster and a service that is configured to have 10 partitions and a target of three replicas.</span></span> <span data-ttu-id="606a1-135">I det här fallet Service Fabric skulle balansera och distribuera replikerna över kluster – och du skulle hamna med två primära [repliker](service-fabric-availability-services.md) per nod.</span><span class="sxs-lookup"><span data-stu-id="606a1-135">In this case, Service Fabric would balance and distribute the replicas across the cluster--and you would end up with two primary [replicas](service-fabric-availability-services.md) per node.</span></span>
<span data-ttu-id="606a1-136">Om du nu behöver skala upp till 10 noder klustret Service Fabric skulle balansera om primärt [repliker](service-fabric-availability-services.md) alla 10 noder.</span><span class="sxs-lookup"><span data-stu-id="606a1-136">If you now need to scale out the cluster to 10 nodes, Service Fabric would rebalance the primary [replicas](service-fabric-availability-services.md) across all 10 nodes.</span></span> <span data-ttu-id="606a1-137">Likaså om du skalas upp till 5 noder skulle Service Fabric balansera om alla repliker över 5 noder.</span><span class="sxs-lookup"><span data-stu-id="606a1-137">Likewise, if you scaled back to 5 nodes, Service Fabric would rebalance all the replicas across the 5 nodes.</span></span>  

<span data-ttu-id="606a1-138">Bild 2 visar fördelningen av 10 partitioner före och efter skalning av klustret.</span><span class="sxs-lookup"><span data-stu-id="606a1-138">Figure 2 shows the distribution of 10 partitions before and after scaling the cluster.</span></span>

![Tillståndskänslig service](./media/service-fabric-concepts-partitioning/partitions.png)

<span data-ttu-id="606a1-140">Därför kan uppnås skalbara eftersom förfrågningar från klienter distribueras mellan datorer, bättre prestanda i programmet och konkurrerar om tillgång till segment av minskas.</span><span class="sxs-lookup"><span data-stu-id="606a1-140">As a result, the scale-out is achieved since requests from clients are distributed across computers, overall performance of the application is improved, and contention on access to chunks of data is reduced.</span></span>

## <a name="plan-for-partitioning"></a><span data-ttu-id="606a1-141">Planera för partitionering</span><span class="sxs-lookup"><span data-stu-id="606a1-141">Plan for partitioning</span></span>
<span data-ttu-id="606a1-142">Innan du implementerar en tjänst, bör du alltid partitioneringsstrategi som behövs för att skala ut.</span><span class="sxs-lookup"><span data-stu-id="606a1-142">Before implementing a service, you should always consider the partitioning strategy that is required to scale out.</span></span> <span data-ttu-id="606a1-143">Det finns olika sätt, men alla fokusera på vad programmet måste uppnå.</span><span class="sxs-lookup"><span data-stu-id="606a1-143">There are different ways, but all of them focus on what the application needs to achieve.</span></span> <span data-ttu-id="606a1-144">Kontexten för den här artikeln ska vi bör du några av de viktigaste aspekterna.</span><span class="sxs-lookup"><span data-stu-id="606a1-144">For the context of this article, let's consider some of the more important aspects.</span></span>

<span data-ttu-id="606a1-145">En bra metod är att tänka strukturen för det tillstånd som måste vara partitionerade som det första steget.</span><span class="sxs-lookup"><span data-stu-id="606a1-145">A good approach is to think about the structure of the state that needs to be partitioned, as the first step.</span></span>

<span data-ttu-id="606a1-146">Låt oss ta ett enkelt exempel.</span><span class="sxs-lookup"><span data-stu-id="606a1-146">Let's take a simple example.</span></span> <span data-ttu-id="606a1-147">Om du skapar en tjänst för en countywide avsökning, kan du skapa en partition för varje ort i delstaten.</span><span class="sxs-lookup"><span data-stu-id="606a1-147">If you were to build a service for a countywide poll, you could create a partition for each city in the county.</span></span> <span data-ttu-id="606a1-148">Sedan kan du lagra rösterna för varje person i stad i partitionen som motsvarar den staden.</span><span class="sxs-lookup"><span data-stu-id="606a1-148">Then, you could store the votes for every person in the city in the partition that corresponds to that city.</span></span> <span data-ttu-id="606a1-149">Bild 3 illustrerar en uppsättning användare och den stad som de finns.</span><span class="sxs-lookup"><span data-stu-id="606a1-149">Figure 3 illustrates a set of people and the city in which they reside.</span></span>

![Enkel partition](./media/service-fabric-concepts-partitioning/cities.png)

<span data-ttu-id="606a1-151">Som befolkningen i orter varierar mycket, kan det sluta med några partitioner som innehåller stora mängder data (till exempel Seattle) och andra partitioner med mycket lite tillstånd (t.ex. Kirkland).</span><span class="sxs-lookup"><span data-stu-id="606a1-151">As the population of cities varies widely, you may end up with some partitions that contain a lot of data (e.g. Seattle) and other partitions with very little state (e.g. Kirkland).</span></span> <span data-ttu-id="606a1-152">Vad är effekten av med partitioner med en ojämn mängder tillstånd?</span><span class="sxs-lookup"><span data-stu-id="606a1-152">So what is the impact of having partitions with uneven amounts of state?</span></span>

<span data-ttu-id="606a1-153">Om du tycker om exemplet igen kan kan du enkelt se att den partition som innehåller rösterna för Seattle kommer mer trafik än Kirkland en.</span><span class="sxs-lookup"><span data-stu-id="606a1-153">If you think about the example again, you can easily see that the partition that holds the votes for Seattle will get more traffic than the Kirkland one.</span></span> <span data-ttu-id="606a1-154">Service Fabric ser till att det handlar om samma antal primära och sekundära repliker på varje nod som standard.</span><span class="sxs-lookup"><span data-stu-id="606a1-154">By default, Service Fabric makes sure that there is about the same number of primary and secondary replicas on each node.</span></span> <span data-ttu-id="606a1-155">Därför kan det sluta med noder som har repliker som fungerar mer nätverkstrafik och andra som hanterar mindre trafik.</span><span class="sxs-lookup"><span data-stu-id="606a1-155">So you may end up with nodes that hold replicas that serve more traffic and others that serve less traffic.</span></span> <span data-ttu-id="606a1-156">Du vill helst undvika varm eller kall punkter så här i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="606a1-156">You would preferably want to avoid hot and cold spots like this in a cluster.</span></span>

<span data-ttu-id="606a1-157">För att undvika detta ska du göra två saker från en partitionering synsätt:</span><span class="sxs-lookup"><span data-stu-id="606a1-157">In order to avoid this, you should do two things, from a partitioning point of view:</span></span>

* <span data-ttu-id="606a1-158">Försök att partitionera tillståndet så att det är jämnt fördelad över alla partitioner.</span><span class="sxs-lookup"><span data-stu-id="606a1-158">Try to partition the state so that it is evenly distributed across all partitions.</span></span>
* <span data-ttu-id="606a1-159">Rapportera belastning från varje replik för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="606a1-159">Report load from each of the replicas for the service.</span></span> <span data-ttu-id="606a1-160">(Mer information om hur checka ut den här artikeln på [mått och Läs in](service-fabric-cluster-resource-manager-metrics.md)).</span><span class="sxs-lookup"><span data-stu-id="606a1-160">(For information on how, check out this article on [Metrics and Load](service-fabric-cluster-resource-manager-metrics.md)).</span></span> <span data-ttu-id="606a1-161">Service Fabric ger möjlighet att rapportera belastning som används av tjänster, till exempel minne eller antal poster.</span><span class="sxs-lookup"><span data-stu-id="606a1-161">Service Fabric provides the capability to report load consumed by services, such as amount of memory or number of records.</span></span> <span data-ttu-id="606a1-162">Utifrån de mätvärden som rapporterats, upptäcker Service Fabric att en del partitioner hanterar högre belastning än andra och balanserar klustret genom att flytta repliker till lämpligare noder så att övergripande ingen nod är överbelastad.</span><span class="sxs-lookup"><span data-stu-id="606a1-162">Based on the metrics reported, Service Fabric detects that some partitions are serving higher loads than others and rebalances the cluster by moving replicas to more suitable nodes, so that overall no node is overloaded.</span></span>

<span data-ttu-id="606a1-163">Du kan ibland vet hur mycket data visas i en given partition.</span><span class="sxs-lookup"><span data-stu-id="606a1-163">Sometimes, you cannot know how much data will be in a given partition.</span></span> <span data-ttu-id="606a1-164">Så att en allmän rekommendation är att båda--först sprids genom användning av en partitioneringsstrategi som data jämnt över partitioner och andra, med reporting belastningen.</span><span class="sxs-lookup"><span data-stu-id="606a1-164">So a general recommendation is to do both--first, by adopting a partitioning strategy that spreads the data evenly across the partitions and second, by reporting load.</span></span>  <span data-ttu-id="606a1-165">Den första metoden förhindrar situationer som beskrivs i röstning exempel, medan andra kan jämna ut temporära skillnader i åtkomst eller belastningsutjämning över tid.</span><span class="sxs-lookup"><span data-stu-id="606a1-165">The first method prevents situations described in the voting example, while the second helps smooth out temporary differences in access or load over time.</span></span>

<span data-ttu-id="606a1-166">En annan aspekt av partition planering är att välja rätt antal partitioner börja med.</span><span class="sxs-lookup"><span data-stu-id="606a1-166">Another aspect of partition planning is to choose the correct number of partitions to begin with.</span></span>
<span data-ttu-id="606a1-167">Från ett Service Fabric-perspektiv finns det inget som förhindrar börjat med ett högre antal partitioner än förväntat för ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="606a1-167">From a Service Fabric perspective, there is nothing that prevents you from starting out with a higher number of partitions than anticipated for your scenario.</span></span>
<span data-ttu-id="606a1-168">I själva verket under förutsättning att det maximala antalet partitioner är en giltig metod.</span><span class="sxs-lookup"><span data-stu-id="606a1-168">In fact, assuming the maximum number of partitions is a valid approach.</span></span>

<span data-ttu-id="606a1-169">I sällsynta fall kan du få behöver fler partitioner än du ursprungligen har valt.</span><span class="sxs-lookup"><span data-stu-id="606a1-169">In rare cases, you may end up needing more partitions than you have initially chosen.</span></span> <span data-ttu-id="606a1-170">Eftersom du inte kan ändra antalet partitioner efteråt, behöver du tillämpa vissa avancerade partition metoder, till exempel skapa en ny service-instans av samma service-typen.</span><span class="sxs-lookup"><span data-stu-id="606a1-170">As you cannot change the partition count after the fact, you would need to apply some advanced partition approaches, such as creating a new service instance of the same service type.</span></span> <span data-ttu-id="606a1-171">Du kan också behöva implementera vissa klientsidan logik som skickar begäranden till rätt tjänstinstansen, baserat på klientsidan kunskap som din klientkod måste underhåll.</span><span class="sxs-lookup"><span data-stu-id="606a1-171">You would also need to implement some client-side logic that routes the requests to the correct service instance, based on client-side knowledge that your client code must maintain.</span></span>

<span data-ttu-id="606a1-172">Ett annat övervägande för partitionering av planering är tillgängliga datorresurser.</span><span class="sxs-lookup"><span data-stu-id="606a1-172">Another consideration for partitioning planning is the available computer resources.</span></span> <span data-ttu-id="606a1-173">När tillståndet behöver komma åt och lagras, är du bundna till följer:</span><span class="sxs-lookup"><span data-stu-id="606a1-173">As the state needs to be accessed and stored, you are bound to follow:</span></span>

* <span data-ttu-id="606a1-174">Gränser för bandbredd i nätverket</span><span class="sxs-lookup"><span data-stu-id="606a1-174">Network bandwidth limits</span></span>
* <span data-ttu-id="606a1-175">System minnesgränserna</span><span class="sxs-lookup"><span data-stu-id="606a1-175">System memory limits</span></span>
* <span data-ttu-id="606a1-176">Disk Lagringsgränser</span><span class="sxs-lookup"><span data-stu-id="606a1-176">Disk storage limits</span></span>

<span data-ttu-id="606a1-177">Så vad händer om du stöter på resursen begränsningar i ett kluster som körs?</span><span class="sxs-lookup"><span data-stu-id="606a1-177">So what happens if you run into resource constraints in a running cluster?</span></span> <span data-ttu-id="606a1-178">Svaret är att du kan helt enkelt skala ut klustret för att anpassa de nya krav.</span><span class="sxs-lookup"><span data-stu-id="606a1-178">The answer is that you can simply scale out the cluster to accommodate the new requirements.</span></span>

<span data-ttu-id="606a1-179">[Kapacitetsplaneringsguiden](service-fabric-capacity-planning.md) ger vägledning att fastställa hur många noder i klustret måste.</span><span class="sxs-lookup"><span data-stu-id="606a1-179">[The capacity planning guide](service-fabric-capacity-planning.md) offers guidance for how to determine how many nodes your cluster needs.</span></span>

## <a name="get-started-with-partitioning"></a><span data-ttu-id="606a1-180">Kom igång med partitionering</span><span class="sxs-lookup"><span data-stu-id="606a1-180">Get started with partitioning</span></span>
<span data-ttu-id="606a1-181">Det här avsnittet beskrivs hur du kommer igång med partitionering din tjänst.</span><span class="sxs-lookup"><span data-stu-id="606a1-181">This section describes how to get started with partitioning your service.</span></span>

<span data-ttu-id="606a1-182">Service Fabric kan välja mellan tre partitionsscheman:</span><span class="sxs-lookup"><span data-stu-id="606a1-182">Service Fabric offers a choice of three partition schemes:</span></span>

* <span data-ttu-id="606a1-183">Låg partitionering (kallas även UniformInt64Partition).</span><span class="sxs-lookup"><span data-stu-id="606a1-183">Ranged partitioning (otherwise known as UniformInt64Partition).</span></span>
* <span data-ttu-id="606a1-184">Namngivna partitionering.</span><span class="sxs-lookup"><span data-stu-id="606a1-184">Named partitioning.</span></span> <span data-ttu-id="606a1-185">Program med hjälp av den här modellen normalt har data som kan vara bucketed, inom en begränsad mängd.</span><span class="sxs-lookup"><span data-stu-id="606a1-185">Applications using this model usually have data that can be bucketed, within a bounded set.</span></span> <span data-ttu-id="606a1-186">Några vanliga exempel datafält som används som namngivna partitionsnycklar skulle vara regioner, postnummer, kundgrupper eller andra företag gränser.</span><span class="sxs-lookup"><span data-stu-id="606a1-186">Some common examples of data fields used as named partition keys would be regions, postal codes, customer groups, or other business boundaries.</span></span>
* <span data-ttu-id="606a1-187">Singleton partitioneras.</span><span class="sxs-lookup"><span data-stu-id="606a1-187">Singleton partitioning.</span></span> <span data-ttu-id="606a1-188">Singleton-partitioner används vanligtvis när tjänsten inte kräver ytterligare routning.</span><span class="sxs-lookup"><span data-stu-id="606a1-188">Singleton partitions are typically used when the service does not require any additional routing.</span></span> <span data-ttu-id="606a1-189">Till exempel använder tillståndslösa tjänster den här partitioneringsschema som standard.</span><span class="sxs-lookup"><span data-stu-id="606a1-189">For example, stateless services use this partitioning scheme by default.</span></span>

<span data-ttu-id="606a1-190">Namnet och scheman som Singleton partitionering är särskilda typer av låg partitioner.</span><span class="sxs-lookup"><span data-stu-id="606a1-190">Named and Singleton partitioning schemes are special forms of ranged partitions.</span></span> <span data-ttu-id="606a1-191">Som standard låg Visual Studio-mallar för Service Fabric-partitionering, som det är den vanligaste.</span><span class="sxs-lookup"><span data-stu-id="606a1-191">By default, the Visual Studio templates for Service Fabric use ranged partitioning, as it is the most common and useful one.</span></span> <span data-ttu-id="606a1-192">Resten av den här artikeln fokuserar på ranged partitioneringsschema.</span><span class="sxs-lookup"><span data-stu-id="606a1-192">The remainder of this article focuses on the ranged partitioning scheme.</span></span>

### <a name="ranged-partitioning-scheme"></a><span data-ttu-id="606a1-193">Låg partitioneringsschema</span><span class="sxs-lookup"><span data-stu-id="606a1-193">Ranged partitioning scheme</span></span>
<span data-ttu-id="606a1-194">Det här används för att ange ett heltalsintervall (som identifieras med en nyckel för låg och hög nyckel) och ett antal partitioner (n).</span><span class="sxs-lookup"><span data-stu-id="606a1-194">This is used to specify an integer range (identified by a low key and high key) and a number of partitions (n).</span></span> <span data-ttu-id="606a1-195">N partitioner, varje ansvarar för en icke-överlappande underintervall av övergripande partitionsnyckelintervallet skapas.</span><span class="sxs-lookup"><span data-stu-id="606a1-195">It creates n partitions, each responsible for a non-overlapping subrange of the overall partition key range.</span></span> <span data-ttu-id="606a1-196">Till exempel en ranged partitioneringsschema med en låg nyckel 0, övre nyckel 99 och antalet 4 skulle skapa fyra partitioner, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="606a1-196">For example, a ranged partitioning scheme with a low key of 0, a high key of 99, and a count of 4 would create four partitions, as shown below.</span></span>

![Intervallet partitionering](./media/service-fabric-concepts-partitioning/range-partitioning.png)

<span data-ttu-id="606a1-198">En vanlig metod är att skapa ett hash-värde baserat på en unik nyckel i datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="606a1-198">A common approach is to create a hash based on a unique key within the data set.</span></span> <span data-ttu-id="606a1-199">Några vanliga exempel nycklar skulle vara vehicle ID-nummer (VIN), en anställnings-ID eller en unik sträng.</span><span class="sxs-lookup"><span data-stu-id="606a1-199">Some common examples of keys would be a vehicle identification number (VIN), an employee ID, or a unique string.</span></span> <span data-ttu-id="606a1-200">Du skulle sedan generera en Hashkod, modulus viktiga intervallet ska användas som din nyckel med hjälp av den här Unik nyckel.</span><span class="sxs-lookup"><span data-stu-id="606a1-200">By using this unique key, you would then generate a hash code, modulus the key range, to use as your key.</span></span> <span data-ttu-id="606a1-201">Du kan ange det tillåtna intervallet för viktiga övre och nedre gränser.</span><span class="sxs-lookup"><span data-stu-id="606a1-201">You can specify the upper and lower bounds of the allowed key range.</span></span>

### <a name="select-a-hash-algorithm"></a><span data-ttu-id="606a1-202">Välj en hash-algoritm</span><span class="sxs-lookup"><span data-stu-id="606a1-202">Select a hash algorithm</span></span>
<span data-ttu-id="606a1-203">En viktig del av hashing är att välja hash-algoritm.</span><span class="sxs-lookup"><span data-stu-id="606a1-203">An important part of hashing is selecting your hash algorithm.</span></span> <span data-ttu-id="606a1-204">En faktor är om målet är att gruppera liknande nycklar nära varandra (ort känsliga hashing)-- eller om aktiviteten ska distribueras brett för alla partitioner (hash-distribution), vilket är mer vanligt.</span><span class="sxs-lookup"><span data-stu-id="606a1-204">A consideration is whether the goal is to group similar keys near each other (locality sensitive hashing)--or if activity should be distributed broadly across all partitions (distribution hashing), which is more common.</span></span>

<span data-ttu-id="606a1-205">Egenskaperna för en fungerande distribution hash-algoritm är att det är enkelt att beräkna den har några kollisioner och nycklar distribueras jämnt.</span><span class="sxs-lookup"><span data-stu-id="606a1-205">The characteristics of a good distribution hashing algorithm are that it is easy to compute, it has few collisions, and it distributes the keys evenly.</span></span> <span data-ttu-id="606a1-206">Ett bra exempel på en effektiv hash-algoritm är den [FNV 1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) hash-algoritm.</span><span class="sxs-lookup"><span data-stu-id="606a1-206">A good example of an efficient hash algorithm is the [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) hash algorithm.</span></span>

<span data-ttu-id="606a1-207">Mer allmän hash-kod algoritmen alternativ är den [Wikipedia sida på hash-funktioner](http://en.wikipedia.org/wiki/Hash_function).</span><span class="sxs-lookup"><span data-stu-id="606a1-207">A good resource for general hash code algorithm choices is the [Wikipedia page on hash functions](http://en.wikipedia.org/wiki/Hash_function).</span></span>

## <a name="build-a-stateful-service-with-multiple-partitions"></a><span data-ttu-id="606a1-208">Skapa en tillståndskänslig tjänst med flera partitioner</span><span class="sxs-lookup"><span data-stu-id="606a1-208">Build a stateful service with multiple partitions</span></span>
<span data-ttu-id="606a1-209">Nu ska vi skapa din första tillförlitliga tillståndskänslig tjänst med flera partitioner.</span><span class="sxs-lookup"><span data-stu-id="606a1-209">Let's create your first reliable stateful service with multiple partitions.</span></span> <span data-ttu-id="606a1-210">I det här exemplet skapar du ett enkelt program där du vill lagra alla efternamn som börjar med samma bokstav i samma partition.</span><span class="sxs-lookup"><span data-stu-id="606a1-210">In this example, you will build a very simple application where you want to store all last names that start with the same letter in the same partition.</span></span>

<span data-ttu-id="606a1-211">Innan du skriva kod som du behöver tänka på partitioner och partitionsnycklar.</span><span class="sxs-lookup"><span data-stu-id="606a1-211">Before you write any code, you need to think about the partitions and partition keys.</span></span> <span data-ttu-id="606a1-212">Du behöver 26 partitioner (en för varje bokstäver i alfabetet) men vad om låg och hög nycklar?</span><span class="sxs-lookup"><span data-stu-id="606a1-212">You need 26 partitions (one for each letter in the alphabet), but what about the low and high keys?</span></span>
<span data-ttu-id="606a1-213">Eftersom vi bokstavligt vill ha en partition per enhetsbeteckning kan vi använder 0 som nyckeln låg och 25 som övre nyckel som varje bokstav är egen.</span><span class="sxs-lookup"><span data-stu-id="606a1-213">As we literally want to have one partition per letter, we can use 0 as the low key and 25 as the high key, as each letter is its own key.</span></span>

> [!NOTE]
> <span data-ttu-id="606a1-214">Detta är en förenklad scenariot i verkligheten distributionen är ojämnt.</span><span class="sxs-lookup"><span data-stu-id="606a1-214">This is a simplified scenario, as in reality the distribution would be uneven.</span></span> <span data-ttu-id="606a1-215">Senaste namn som börjar med bokstäver ”S” eller ”M” är vanligare än de som börjar med ”X” eller ”Y”.</span><span class="sxs-lookup"><span data-stu-id="606a1-215">Last names starting with the letters "S" or "M" are more common than the ones starting with "X" or "Y".</span></span>
> 
> 

1. <span data-ttu-id="606a1-216">Öppna **Visual Studio** > **filen** > **nya** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="606a1-216">Open **Visual Studio** > **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="606a1-217">I den **nytt projekt** dialogrutan Välj Service Fabric-programmet.</span><span class="sxs-lookup"><span data-stu-id="606a1-217">In the **New Project** dialog box, choose the Service Fabric application.</span></span>
3. <span data-ttu-id="606a1-218">Anropa projektet ”AlphabetPartitions”.</span><span class="sxs-lookup"><span data-stu-id="606a1-218">Call the project "AlphabetPartitions".</span></span>
4. <span data-ttu-id="606a1-219">I den **skapar du en tjänst** dialogrutan Välj **Stateful** tjänst och kalla den ”Alphabet.Processing” som visas i bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="606a1-219">In the **Create a Service** dialog box, choose **Stateful** service and call it "Alphabet.Processing" as shown in the image below.</span></span>
       <span data-ttu-id="606a1-220">![Dialogrutan Ny tjänst i Visual Studio][1]</span><span class="sxs-lookup"><span data-stu-id="606a1-220">![New service dialog in Visual Studio][1]</span></span>

  <!--  ![Stateful service screenshot](./media/service-fabric-concepts-partitioning/createstateful.png)-->

5. <span data-ttu-id="606a1-221">Ange antalet partitioner.</span><span class="sxs-lookup"><span data-stu-id="606a1-221">Set the number of partitions.</span></span> <span data-ttu-id="606a1-222">Öppna filen Applicationmanifest.xml finns i mappen ApplicationPackageRoot AlphabetPartitions-projektet och uppdatera parametern Processing_PartitionCount-26 enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="606a1-222">Open the Applicationmanifest.xml file located in the ApplicationPackageRoot folder of the AlphabetPartitions project and update the parameter Processing_PartitionCount to 26 as shown below.</span></span>
   
    ```xml
    <Parameter Name="Processing_PartitionCount" DefaultValue="26" />
    ```
   
    <span data-ttu-id="606a1-223">Du måste också uppdatera egenskaperna LowKey och HighKey för elementet StatefulService i ApplicationManifest.xml enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="606a1-223">You also need to update the LowKey and HighKey properties of the StatefulService element in the ApplicationManifest.xml as shown below.</span></span>
   
    ```xml
    <Service Name="Processing">
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="[Processing_TargetReplicaSetSize]" MinReplicaSetSize="[Processing_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Processing_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    ```
6. <span data-ttu-id="606a1-224">För tjänsten är tillgänglig, öppna en slutpunkt på en port genom att lägga till slutpunktselementet i ServiceManifest.xml (finns i mappen PackageRoot) för tjänsten Alphabet.Processing enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="606a1-224">For the service to be accessible, open up an endpoint on a port by adding the endpoint element of ServiceManifest.xml (located in the PackageRoot folder) for the Alphabet.Processing service as shown below:</span></span>
   
    ```xml
    <Endpoint Name="ProcessingServiceEndpoint" Port="8089" Protocol="http" Type="Internal" />
    ```
   
    <span data-ttu-id="606a1-225">Tjänsten är nu konfigurerad för att lyssna på en intern slutpunkt med 26 partitioner.</span><span class="sxs-lookup"><span data-stu-id="606a1-225">Now the service is configured to listen to an internal endpoint with 26 partitions.</span></span>
7. <span data-ttu-id="606a1-226">Sedan måste åsidosätta den `CreateServiceReplicaListeners()` metoden i klassen bearbetning.</span><span class="sxs-lookup"><span data-stu-id="606a1-226">Next, you need to override the `CreateServiceReplicaListeners()` method of the Processing class.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="606a1-227">För det här exemplet förutsätter vi att du använder en enkel HttpCommunicationListener.</span><span class="sxs-lookup"><span data-stu-id="606a1-227">For this sample, we assume that you are using a simple HttpCommunicationListener.</span></span> <span data-ttu-id="606a1-228">Mer information om tillförlitlig kommunikation finns [tjänsten tillförlitlig kommunikation modellen](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="606a1-228">For more information on reliable service communication, see [The Reliable Service communication model](service-fabric-reliable-services-communication.md).</span></span>
   > 
   > 
8. <span data-ttu-id="606a1-229">En rekommenderad mönster för URL: en som en replik som lyssnar på är följande format: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.</span><span class="sxs-lookup"><span data-stu-id="606a1-229">A recommended pattern for the URL that a replica listens on is the following format: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.</span></span>
    <span data-ttu-id="606a1-230">Så att du vill konfigurera din kommunikation lyssnare för att lyssna på rätt slutpunkter och med det här mönstret.</span><span class="sxs-lookup"><span data-stu-id="606a1-230">So you want to configure your communication listener to listen on the correct endpoints and with this pattern.</span></span>
   
    <span data-ttu-id="606a1-231">Flera repliker av den här tjänsten kan finnas på samma dator, så att den här adressen måste vara unikt på repliken.</span><span class="sxs-lookup"><span data-stu-id="606a1-231">Multiple replicas of this service may be hosted on the same computer, so this address needs to be unique to the replica.</span></span> <span data-ttu-id="606a1-232">Det är därför partitions-ID + replik-ID är URL-adressen.</span><span class="sxs-lookup"><span data-stu-id="606a1-232">This is why   partition ID + replica ID are in the URL.</span></span> <span data-ttu-id="606a1-233">HttpListener kan lyssna på flera adresser på samma port som URL-prefixet är unikt.</span><span class="sxs-lookup"><span data-stu-id="606a1-233">HttpListener can listen on multiple addresses on the same port as long as the URL prefix    is unique.</span></span>
   
    <span data-ttu-id="606a1-234">Extra GUID är det för en avancerad fall där sekundära repliker också lyssna efter begäranden i skrivskyddat läge.</span><span class="sxs-lookup"><span data-stu-id="606a1-234">The extra GUID is there for an advanced case where secondary replicas also listen for read-only requests.</span></span> <span data-ttu-id="606a1-235">När så är fallet, som du vill kontrollera att en ny unik adress används vid övergång från primär till sekundär för att tvinga klienter att matcha adressen igen.</span><span class="sxs-lookup"><span data-stu-id="606a1-235">When that's the case, you want to make sure that a new unique address is used when transitioning from primary to secondary to force clients to re-resolve the address.</span></span> <span data-ttu-id="606a1-236">”+” används som den här adressen så att repliken lyssnar på alla tillgängliga värdar (IP, FQDM localhost, osv.) Koden nedan visar ett exempel.</span><span class="sxs-lookup"><span data-stu-id="606a1-236">'+' is used as the address here so that the replica listens on all available hosts (IP, FQDM, localhost, etc.) The code below shows an example.</span></span>
   
    ```CSharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
         return new[] { new ServiceReplicaListener(context => this.CreateInternalListener(context))};
    }
    private ICommunicationListener CreateInternalListener(ServiceContext context)
    {
   
         EndpointResourceDescription internalEndpoint = context.CodePackageActivationContext.GetEndpoint("ProcessingServiceEndpoint");
         string uriPrefix = String.Format(
                "{0}://+:{1}/{2}/{3}-{4}/",
                internalEndpoint.Protocol,
                internalEndpoint.Port,
                context.PartitionId,
                context.ReplicaOrInstanceId,
                Guid.NewGuid());
   
         string nodeIP = FabricRuntime.GetNodeContext().IPAddressOrFQDN;
   
         string uriPublished = uriPrefix.Replace("+", nodeIP);
         return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInternalRequest);
    }
    ```
   
    <span data-ttu-id="606a1-237">Det är också värt att nämna att publicerade URL: en är skiljer sig från lyssnande URL-prefixet.</span><span class="sxs-lookup"><span data-stu-id="606a1-237">It's also worth noting that the published URL is slightly different from the listening URL prefix.</span></span>
    <span data-ttu-id="606a1-238">URL: en lyssnande ges till HttpListener.</span><span class="sxs-lookup"><span data-stu-id="606a1-238">The listening URL is given to HttpListener.</span></span> <span data-ttu-id="606a1-239">Publicerade URL: en är den URL som publicerats till Service Fabric Naming Service, som används för identifiering av tjänst.</span><span class="sxs-lookup"><span data-stu-id="606a1-239">The published URL is the URL that is published to the Service Fabric Naming Service, which is used for service discovery.</span></span> <span data-ttu-id="606a1-240">Klienter begär den här adressen genom att discovery-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="606a1-240">Clients will ask for this address through that discovery service.</span></span> <span data-ttu-id="606a1-241">Den adress som klienterna hämta måste ha en faktisk IP eller FQDN för noden för att kunna ansluta.</span><span class="sxs-lookup"><span data-stu-id="606a1-241">The address that clients get needs to have the actual IP or FQDN of the node in order to connect.</span></span> <span data-ttu-id="606a1-242">Så du behöver ersätta '+' med nodens IP eller FQDN som ovan.</span><span class="sxs-lookup"><span data-stu-id="606a1-242">So you need to replace '+' with the node's IP or FQDN as shown above.</span></span>
9. <span data-ttu-id="606a1-243">Det sista steget är att lägga till standardbearbetningslogiken i tjänsten som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="606a1-243">The last step is to add the processing logic to the service as shown below.</span></span>
   
    ```CSharp
    private async Task ProcessInternalRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        string output = null;
        string user = context.Request.QueryString["lastname"].ToString();
   
        try
        {
            output = await this.AddUserAsync(user);
        }
        catch (Exception ex)
        {
            output = ex.Message;
        }
   
        using (HttpListenerResponse response = context.Response)
        {
            if (output != null)
            {
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    private async Task<string> AddUserAsync(string user)
    {
        IReliableDictionary<String, String> dictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<String, String>>("dictionary");
   
        using (ITransaction tx = this.StateManager.CreateTransaction())
        {
            bool addResult = await dictionary.TryAddAsync(tx, user.ToUpperInvariant(), user);
   
            await tx.CommitAsync();
   
            return String.Format(
                "User {0} {1}",
                user,
                addResult ? "sucessfully added" : "already exists");
        }
    }
    ```
   
    <span data-ttu-id="606a1-244">`ProcessInternalRequest`läser värdena för frågesträngparametern som används för att anropa partition och anrop `AddUserAsync` att lägga till efternamn tillförlitliga ordlistan `dictionary`.</span><span class="sxs-lookup"><span data-stu-id="606a1-244">`ProcessInternalRequest` reads the values of the query string parameter used to call the partition and calls `AddUserAsync` to add the lastname to the reliable dictionary `dictionary`.</span></span>
10. <span data-ttu-id="606a1-245">Lägg till en tillståndslös tjänst projekt för att se hur du kan anropa en viss partition.</span><span class="sxs-lookup"><span data-stu-id="606a1-245">Let's add a stateless service to the project to see how you can call a particular partition.</span></span>
    
    <span data-ttu-id="606a1-246">Den här tjänsten fungerar som ett enkelt webbgränssnitt som accepterar efternamn som en frågesträngsparameter anger Partitionsnyckeln och skickar det till tjänsten Alphabet.Processing för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="606a1-246">This service serves as a simple web interface that accepts the lastname as a query string parameter, determines the partition key, and sends it to the Alphabet.Processing service for processing.</span></span>
11. <span data-ttu-id="606a1-247">I den **skapar du en tjänst** dialogrutan Välj **Stateless** tjänst och kalla den ”Alphabet.Web” enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="606a1-247">In the **Create a Service** dialog box, choose **Stateless** service and call it "Alphabet.Web" as shown below.</span></span>
    
    ![Skärmbild av tillståndslösa tjänsten](./media/service-fabric-concepts-partitioning/createnewstateless.png)<span data-ttu-id="606a1-249">.</span><span class="sxs-lookup"><span data-stu-id="606a1-249">.</span></span>
12. <span data-ttu-id="606a1-250">Uppdatera slutpunktsinformationen i ServiceManifest.xml Alphabet.WebApi-tjänsten för att öppna en port som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="606a1-250">Update the endpoint information in the ServiceManifest.xml of the Alphabet.WebApi service to open up a port as shown below.</span></span>
    
    ```xml
    <Endpoint Name="WebApiServiceEndpoint" Protocol="http" Port="8081"/>
    ```
13. <span data-ttu-id="606a1-251">Du måste returnera en samling med ServiceInstanceListeners i klassen Web.</span><span class="sxs-lookup"><span data-stu-id="606a1-251">You need to return a collection of ServiceInstanceListeners in the class Web.</span></span> <span data-ttu-id="606a1-252">Igen, kan du välja att implementera en enkel HttpCommunicationListener.</span><span class="sxs-lookup"><span data-stu-id="606a1-252">Again, you can choose to implement a simple HttpCommunicationListener.</span></span>
    
    ```CSharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] {new ServiceInstanceListener(context => this.CreateInputListener(context))};
    }
    private ICommunicationListener CreateInputListener(ServiceContext context)
    {
        // Service instance's URL is the node's IP & desired port
        EndpointResourceDescription inputEndpoint = context.CodePackageActivationContext.GetEndpoint("WebApiServiceEndpoint")
        string uriPrefix = String.Format("{0}://+:{1}/alphabetpartitions/", inputEndpoint.Protocol, inputEndpoint.Port);
        var uriPublished = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
        return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInputRequest);
    }
    ```
14. <span data-ttu-id="606a1-253">Nu måste du implementera logik för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="606a1-253">Now you need to implement the processing logic.</span></span> <span data-ttu-id="606a1-254">HttpCommunicationListener anropen `ProcessInputRequest` när en begäran kommer.</span><span class="sxs-lookup"><span data-stu-id="606a1-254">The HttpCommunicationListener calls `ProcessInputRequest` when a request comes in.</span></span> <span data-ttu-id="606a1-255">Så Låt oss och Lägg till koden nedan.</span><span class="sxs-lookup"><span data-stu-id="606a1-255">So let's go ahead and add the code below.</span></span>
    
    ```CSharp
    private async Task ProcessInputRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        String output = null;
        try
        {
            string lastname = context.Request.QueryString["lastname"];
            char firstLetterOfLastName = lastname.First();
            ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    
            ResolvedServicePartition partition = await this.servicePartitionResolver.ResolveAsync(alphabetServiceUri, partitionKey, cancelRequest);
            ResolvedServiceEndpoint ep = partition.GetEndpoint();
    
            JObject addresses = JObject.Parse(ep.Address);
            string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
            UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
            primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
            string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    
            output = String.Format(
                    "Result: {0}. <p>Partition key: '{1}' generated from the first letter '{2}' of input value '{3}'. <br>Processing service partition ID: {4}. <br>Processing service replica address: {5}",
                    result,
                    partitionKey,
                    firstLetterOfLastName,
                    lastname,
                    partition.Info.Id,
                    primaryReplicaAddress);
        }
        catch (Exception ex) { output = ex.Message; }
    
        using (var response = context.Response)
        {
            if (output != null)
            {
                output = output + "added to Partition: " + primaryReplicaAddress;
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    ```
    
    <span data-ttu-id="606a1-256">Nu ska vi gå igenom den steg för steg.</span><span class="sxs-lookup"><span data-stu-id="606a1-256">Let's walk through it step by step.</span></span> <span data-ttu-id="606a1-257">Koden läser den första bokstaven i frågesträngparametern `lastname` till char.</span><span class="sxs-lookup"><span data-stu-id="606a1-257">The code reads the first letter of the query string parameter `lastname` into a char.</span></span> <span data-ttu-id="606a1-258">Sedan den avgör Partitionsnyckeln för den här bokstaven genom att subtrahera det hexadecimala värdet av `A` från det hexadecimala värdet för senaste namnen med versal.</span><span class="sxs-lookup"><span data-stu-id="606a1-258">Then, it determines the partition key for this letter by subtracting the hexadecimal value of `A` from the hexadecimal value of the last names' first letter.</span></span>
    
    ```CSharp
    string lastname = context.Request.QueryString["lastname"];
    char firstLetterOfLastName = lastname.First();
    ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    ```
    
    <span data-ttu-id="606a1-259">Kom ihåg att det här exemplet använder vi 26 partitioner med en partitionsnyckel per partition.</span><span class="sxs-lookup"><span data-stu-id="606a1-259">Remember, for this example, we are using 26 partitions with one partition key per partition.</span></span>
    <span data-ttu-id="606a1-260">Nu ska vi får service partitionen `partition` för den här nyckeln med hjälp av den `ResolveAsync` metod på den `servicePartitionResolver` objekt.</span><span class="sxs-lookup"><span data-stu-id="606a1-260">Next, we obtain the service partition `partition` for this key by using the `ResolveAsync` method on the `servicePartitionResolver` object.</span></span> <span data-ttu-id="606a1-261">`servicePartitionResolver`definieras som</span><span class="sxs-lookup"><span data-stu-id="606a1-261">`servicePartitionResolver` is defined as</span></span>
    
    ```CSharp
    private readonly ServicePartitionResolver servicePartitionResolver = ServicePartitionResolver.GetDefault();
    ```
    
    <span data-ttu-id="606a1-262">Den `ResolveAsync` metoden tar tjänsten URI, Partitionsnyckeln och en annullering token som parametrar.</span><span class="sxs-lookup"><span data-stu-id="606a1-262">The `ResolveAsync` method takes the service URI, the partition key, and a cancellation token as parameters.</span></span> <span data-ttu-id="606a1-263">Tjänsten URI för behandling av tjänsten är `fabric:/AlphabetPartitions/Processing`.</span><span class="sxs-lookup"><span data-stu-id="606a1-263">The service URI for the processing service is `fabric:/AlphabetPartitions/Processing`.</span></span> <span data-ttu-id="606a1-264">Kan gå vi slutpunkten för partitionen.</span><span class="sxs-lookup"><span data-stu-id="606a1-264">Next, we get the endpoint of the partition.</span></span>
    
    ```CSharp
    ResolvedServiceEndpoint ep = partition.GetEndpoint()
    ```
    
    <span data-ttu-id="606a1-265">Slutligen vi skapar slutpunkts-URL plus frågesträngen och anropa behandling av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="606a1-265">Finally, we build the endpoint URL plus the querystring and call the processing service.</span></span>
    
    ```CSharp
    JObject addresses = JObject.Parse(ep.Address);
    string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
    UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
    primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
    string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    ```
    
    <span data-ttu-id="606a1-266">När bearbetningen är klar, vi skriva utdata tillbaka.</span><span class="sxs-lookup"><span data-stu-id="606a1-266">Once the processing is done, we write the output back.</span></span>
15. <span data-ttu-id="606a1-267">Det sista steget är att testa tjänsten.</span><span class="sxs-lookup"><span data-stu-id="606a1-267">The last step is to test the service.</span></span> <span data-ttu-id="606a1-268">Visual Studio använder programmet parametrar för lokal och distribution.</span><span class="sxs-lookup"><span data-stu-id="606a1-268">Visual Studio uses application parameters for local and cloud deployment.</span></span> <span data-ttu-id="606a1-269">Om du vill testa tjänsten med 26 partitioner lokalt, måste du uppdatera den `Local.xml` filen i mappen ApplicationParameters AlphabetPartitions-projektet enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="606a1-269">To test the service with 26 partitions locally, you need to update the `Local.xml` file in the ApplicationParameters folder of the AlphabetPartitions project as shown below:</span></span>
    
    ```xml
    <Parameters>
      <Parameter Name="Processing_PartitionCount" Value="26" />
      <Parameter Name="WebApi_InstanceCount" Value="1" />
    </Parameters>
    ```
16. <span data-ttu-id="606a1-270">Du kan kontrollera tjänsten och alla dess partitioner i Service Fabric Explorer när du är klar med distributionen.</span><span class="sxs-lookup"><span data-stu-id="606a1-270">Once you finish deployment, you can check the service and all of its partitions in the Service Fabric Explorer.</span></span>
    
    ![Service Fabric Explorer skärmbild](./media/service-fabric-concepts-partitioning/sfxpartitions.png)
17. <span data-ttu-id="606a1-272">I en webbläsare, kan du testa partitionering logiken genom att ange `http://localhost:8081/?lastname=somename`.</span><span class="sxs-lookup"><span data-stu-id="606a1-272">In a browser, you can test the partitioning logic by entering `http://localhost:8081/?lastname=somename`.</span></span> <span data-ttu-id="606a1-273">Du ser att varje efternamn som börjar med samma enhetsbeteckning som lagras i samma partition.</span><span class="sxs-lookup"><span data-stu-id="606a1-273">You will see that each last name that starts with the same letter is being stored in the same partition.</span></span>
    
    ![Skärmbild för webbläsare](./media/service-fabric-concepts-partitioning/samplerunning.png)

<span data-ttu-id="606a1-275">Källkoden hela det här exemplet är tillgängligt på [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span><span class="sxs-lookup"><span data-stu-id="606a1-275">The entire source code of the sample is available on [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span></span>

## <a name="next-steps"></a><span data-ttu-id="606a1-276">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="606a1-276">Next steps</span></span>
<span data-ttu-id="606a1-277">Information om Service Fabric-begrepp finns i:</span><span class="sxs-lookup"><span data-stu-id="606a1-277">For information on Service Fabric concepts, see the following:</span></span>

* [<span data-ttu-id="606a1-278">Tillgänglighet för Service Fabric-tjänster</span><span class="sxs-lookup"><span data-stu-id="606a1-278">Availability of Service Fabric services</span></span>](service-fabric-availability-services.md)
* [<span data-ttu-id="606a1-279">Skalbarheten för Service Fabric-tjänster</span><span class="sxs-lookup"><span data-stu-id="606a1-279">Scalability of Service Fabric services</span></span>](service-fabric-concepts-scalability.md)
* [<span data-ttu-id="606a1-280">Kapacitetsplanering för Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="606a1-280">Capacity planning for Service Fabric applications</span></span>](service-fabric-capacity-planning.md)

[wikipartition]: https://en.wikipedia.org/wiki/Partition_(database)

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png