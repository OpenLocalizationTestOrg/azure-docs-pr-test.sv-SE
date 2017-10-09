---
title: "aaaPartitioning Service Fabric-tjänster | Microsoft Docs"
description: "Beskriver hur toopartition Service Fabric tillståndskänsliga tjänster. Partitioner möjliggör datalagring på hello lokala datorer så att data och beräkning kan skalas tillsammans."
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
ms.openlocfilehash: 6ead48716c08f4212535202ee69d169067d5c6d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="partition-service-fabric-reliable-services"></a><span data-ttu-id="90cc2-104">Partitionen Service Fabric reliable services</span><span class="sxs-lookup"><span data-stu-id="90cc2-104">Partition Service Fabric reliable services</span></span>
<span data-ttu-id="90cc2-105">Den här artikeln innehåller en introduktion toohello grundläggande begrepp för partitionering tillförlitliga Azure Service Fabric-tjänster.</span><span class="sxs-lookup"><span data-stu-id="90cc2-105">This article provides an introduction toohello basic concepts of partitioning Azure Service Fabric reliable services.</span></span> <span data-ttu-id="90cc2-106">hello källkoden som används i hello artikeln finns också på [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span><span class="sxs-lookup"><span data-stu-id="90cc2-106">hello source code used in hello article is also available on [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span></span>

## <a name="partitioning"></a><span data-ttu-id="90cc2-107">Partitionering</span><span class="sxs-lookup"><span data-stu-id="90cc2-107">Partitioning</span></span>
<span data-ttu-id="90cc2-108">Partitionering är inte unikt tooService Fabric.</span><span class="sxs-lookup"><span data-stu-id="90cc2-108">Partitioning is not unique tooService Fabric.</span></span> <span data-ttu-id="90cc2-109">Det är faktiskt en core mönster för att skapa skalbara tjänster.</span><span class="sxs-lookup"><span data-stu-id="90cc2-109">In fact, it is a core pattern of building scalable services.</span></span> <span data-ttu-id="90cc2-110">I en bredare mening vi tror om partitionering som ett koncept för att dela tillstånd (data) och beräkna i mindre tillgänglig enheter tooimprove skalbarhet och prestanda.</span><span class="sxs-lookup"><span data-stu-id="90cc2-110">In a broader sense, we can think about partitioning as a concept of dividing state (data) and compute into smaller accessible units tooimprove scalability and performance.</span></span> <span data-ttu-id="90cc2-111">Ett välkänt formulär partitionering är [Datapartitionering][wikipartition], även kallad horisontell partitionering.</span><span class="sxs-lookup"><span data-stu-id="90cc2-111">A well-known form of partitioning is [data partitioning][wikipartition], also known as sharding.</span></span>

### <a name="partition-service-fabric-stateless-services"></a><span data-ttu-id="90cc2-112">Tillståndslösa tjänster för Service Fabric för partition</span><span class="sxs-lookup"><span data-stu-id="90cc2-112">Partition Service Fabric stateless services</span></span>
<span data-ttu-id="90cc2-113">Du kan se om en partition är en logisk enhet som innehåller en eller flera instanser av en tjänst för tillståndslösa tjänster.</span><span class="sxs-lookup"><span data-stu-id="90cc2-113">For stateless services, you can think about a partition being a logical unit that contains one or more instances of a service.</span></span> <span data-ttu-id="90cc2-114">Bild 1 visar tillståndslösa tjänsten med fem instanser distribuerade i ett kluster med en partition.</span><span class="sxs-lookup"><span data-stu-id="90cc2-114">Figure 1 shows a stateless service with five instances distributed across a cluster using one partition.</span></span>

![Tillståndslösa tjänsten](./media/service-fabric-concepts-partitioning/statelessinstances.png)

<span data-ttu-id="90cc2-116">Det finns två typer av tillståndslösa tjänstelösningar verkligen.</span><span class="sxs-lookup"><span data-stu-id="90cc2-116">There are really two types of stateless service solutions.</span></span> <span data-ttu-id="90cc2-117">hello först är en en tjänst som kvarstår dess tillstånd externt, till exempel i en Azure SQL database (till exempel en webbplats som lagrar hello sessionsinformation och data).</span><span class="sxs-lookup"><span data-stu-id="90cc2-117">hello first one is a service that persists its state externally, for example in an Azure SQL database (like a website that stores hello session information and data).</span></span> <span data-ttu-id="90cc2-118">hello är andra endast beräkning-tjänster (till exempel Kalkylatorn eller image thumbnailing) som inte hanterar några beständiga tillstånd.</span><span class="sxs-lookup"><span data-stu-id="90cc2-118">hello second one is computation-only services (like a calculator or image thumbnailing) that do not manage any persistent state.</span></span>

<span data-ttu-id="90cc2-119">I antingen fallet partitionering tillståndslösa tjänsten är ett mycket sällsynt scenario--skalbarhet och tillgänglighet uppnås normalt genom att lägga till flera instanser.</span><span class="sxs-lookup"><span data-stu-id="90cc2-119">In either case, partitioning a stateless service is a very rare scenario--scalability and availability are normally achieved by adding more instances.</span></span> <span data-ttu-id="90cc2-120">hello endast-tid som du vill tooconsider flera partitioner för tillståndslösa tjänstinstanser är när du behöver toomeet särskilda routning begäranden.</span><span class="sxs-lookup"><span data-stu-id="90cc2-120">hello only time you want tooconsider multiple partitions for stateless service instances is when you need toomeet special routing requests.</span></span>

<span data-ttu-id="90cc2-121">Exempelvis bör du fall där användare med ID: N i ett visst intervall endast hanteras av en viss tjänst-instans.</span><span class="sxs-lookup"><span data-stu-id="90cc2-121">As an example, consider a case where users with IDs in a certain range should only be served by a particular service instance.</span></span> <span data-ttu-id="90cc2-122">Ett annat exempel på när du gick partitionera en tillståndslös tjänst är när du har en verkligen partitionerade serverdel (t.ex. ett delat SQL database) och du vill toocontrol vilken tjänstinstans ska skriva toohello databasen Fragmentera-- eller utföra andra förberedelser arbete i hello tillståndslösa tjänsten som kräver hello samma partitionering information som används i hello backend.</span><span class="sxs-lookup"><span data-stu-id="90cc2-122">Another example of when you could partition a stateless service is when you have a truly partitioned backend (e.g. a sharded SQL database) and you want toocontrol which service instance should write toohello database shard--or perform other preparation work within hello stateless service that requires hello same partitioning information as is used in hello backend.</span></span> <span data-ttu-id="90cc2-123">Dessa typer av scenarier kan också lösas på olika sätt och kräver inte nödvändigtvis service partitionering.</span><span class="sxs-lookup"><span data-stu-id="90cc2-123">Those types of scenarios can also be solved in different ways and do not necessarily require service partitioning.</span></span>

<span data-ttu-id="90cc2-124">hello resten av den här genomgången fokuserar på tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="90cc2-124">hello remainder of this walkthrough focuses on stateful services.</span></span>

### <a name="partition-service-fabric-stateful-services"></a><span data-ttu-id="90cc2-125">Tillståndskänsliga tjänster för Service Fabric för partition</span><span class="sxs-lookup"><span data-stu-id="90cc2-125">Partition Service Fabric stateful services</span></span>
<span data-ttu-id="90cc2-126">Service Fabric gör det enkelt toodevelop skalbara tillståndskänsliga tjänster genom att erbjuda förstklassig sätt toopartition tillstånd (data).</span><span class="sxs-lookup"><span data-stu-id="90cc2-126">Service Fabric makes it easy toodevelop scalable stateful services by offering a first-class way toopartition state (data).</span></span> <span data-ttu-id="90cc2-127">Begreppsmässigt, du kan se om en partition av en tillståndskänslig service som en skalningsenhet som är mycket pålitlig via [repliker](service-fabric-availability-services.md) som distribueras och balanserat över hello noder i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="90cc2-127">Conceptually, you can think about a partition of a stateful service as a scale unit that is highly reliable through [replicas](service-fabric-availability-services.md) that are distributed and balanced across hello nodes in a cluster.</span></span>

<span data-ttu-id="90cc2-128">Partitionering hello gäller Service Fabric tillståndskänsliga tjänster refererar toohello process för att fastställa att en viss tjänst partition är ansvarig för en del av hello hello tjänstens fullständiga tillstånd.</span><span class="sxs-lookup"><span data-stu-id="90cc2-128">Partitioning in hello context of Service Fabric stateful services refers toohello process of determining that a particular service partition is responsible for a portion of hello complete state of hello service.</span></span> <span data-ttu-id="90cc2-129">(Som tidigare nämnts, en partition är en uppsättning [repliker](service-fabric-availability-services.md)).</span><span class="sxs-lookup"><span data-stu-id="90cc2-129">(As mentioned before, a partition is a set of [replicas](service-fabric-availability-services.md)).</span></span> <span data-ttu-id="90cc2-130">En fantastiska med Service Fabric är att den placerar hello partitioner på olika noder.</span><span class="sxs-lookup"><span data-stu-id="90cc2-130">A great thing about Service Fabric is that it places hello partitions on different nodes.</span></span> <span data-ttu-id="90cc2-131">Detta innebär att de toogrow tooa nod resursgräns.</span><span class="sxs-lookup"><span data-stu-id="90cc2-131">This allows them toogrow tooa node's resource limit.</span></span> <span data-ttu-id="90cc2-132">När hello data behöver växa, partitioner växer och Service Fabric balanserar partitioner mellan noder.</span><span class="sxs-lookup"><span data-stu-id="90cc2-132">As hello data needs grow, partitions grow, and Service Fabric rebalances partitions across nodes.</span></span> <span data-ttu-id="90cc2-133">Detta säkerställer att hello fortsatt effektiv användning av maskinvaruresurser.</span><span class="sxs-lookup"><span data-stu-id="90cc2-133">This ensures hello continued efficient use of hardware resources.</span></span>

<span data-ttu-id="90cc2-134">toogive du exempelvis säger att du börjar med ett kluster med 5 och en tjänst som är konfigurerade toohave 10 partitioner och ett mål för tre repliker.</span><span class="sxs-lookup"><span data-stu-id="90cc2-134">toogive you an example, say you start with a 5-node cluster and a service that is configured toohave 10 partitions and a target of three replicas.</span></span> <span data-ttu-id="90cc2-135">I det här fallet Service Fabric skulle balansera och distribuera hello repliker över hello kluster – och du skulle hamna med två primära [repliker](service-fabric-availability-services.md) per nod.</span><span class="sxs-lookup"><span data-stu-id="90cc2-135">In this case, Service Fabric would balance and distribute hello replicas across hello cluster--and you would end up with two primary [replicas](service-fabric-availability-services.md) per node.</span></span>
<span data-ttu-id="90cc2-136">Om du behöver nu tooscale ut hello too10 klusternoder, Service Fabric skulle balansera hello primära [repliker](service-fabric-availability-services.md) alla 10 noder.</span><span class="sxs-lookup"><span data-stu-id="90cc2-136">If you now need tooscale out hello cluster too10 nodes, Service Fabric would rebalance hello primary [replicas](service-fabric-availability-services.md) across all 10 nodes.</span></span> <span data-ttu-id="90cc2-137">Likaså om du skala tillbaka too5 noder skulle Service Fabric balansera om alla hello repliker över hello 5 noder.</span><span class="sxs-lookup"><span data-stu-id="90cc2-137">Likewise, if you scaled back too5 nodes, Service Fabric would rebalance all hello replicas across hello 5 nodes.</span></span>  

<span data-ttu-id="90cc2-138">Bild 2 visar hello distribution av 10 partitioner före och efter skalning hello klustret.</span><span class="sxs-lookup"><span data-stu-id="90cc2-138">Figure 2 shows hello distribution of 10 partitions before and after scaling hello cluster.</span></span>

![Tillståndskänslig service](./media/service-fabric-concepts-partitioning/partitions.png)

<span data-ttu-id="90cc2-140">Därför kan uppnås hello skalbar eftersom förfrågningar från klienter distribueras mellan datorer, bättre prestanda i programmet hello och minskar konkurrens på toochunks för åtkomst av data.</span><span class="sxs-lookup"><span data-stu-id="90cc2-140">As a result, hello scale-out is achieved since requests from clients are distributed across computers, overall performance of hello application is improved, and contention on access toochunks of data is reduced.</span></span>

## <a name="plan-for-partitioning"></a><span data-ttu-id="90cc2-141">Planera för partitionering</span><span class="sxs-lookup"><span data-stu-id="90cc2-141">Plan for partitioning</span></span>
<span data-ttu-id="90cc2-142">Innan du implementerar en tjänst bör du alltid hello strategi som är nödvändiga tooscale out partitionering. Det finns olika sätt, men alla fokusera på vad hello-programmet måste tooachieve.</span><span class="sxs-lookup"><span data-stu-id="90cc2-142">Before implementing a service, you should always consider hello partitioning strategy that is required tooscale out. There are different ways, but all of them focus on what hello application needs tooachieve.</span></span> <span data-ttu-id="90cc2-143">Hello-kontexten för den här artikeln ska vi tänka på hello fler viktiga aspekter.</span><span class="sxs-lookup"><span data-stu-id="90cc2-143">For hello context of this article, let's consider some of hello more important aspects.</span></span>

<span data-ttu-id="90cc2-144">En bra metod är toothink om hello struktur hello tillstånd som behöver toobe partitioneras som hello första steget.</span><span class="sxs-lookup"><span data-stu-id="90cc2-144">A good approach is toothink about hello structure of hello state that needs toobe partitioned, as hello first step.</span></span>

<span data-ttu-id="90cc2-145">Låt oss ta ett enkelt exempel.</span><span class="sxs-lookup"><span data-stu-id="90cc2-145">Let's take a simple example.</span></span> <span data-ttu-id="90cc2-146">Om du toobuild en tjänst för en countywide avsökning, kan du skapa en partition för varje ort i hello region.</span><span class="sxs-lookup"><span data-stu-id="90cc2-146">If you were toobuild a service for a countywide poll, you could create a partition for each city in hello county.</span></span> <span data-ttu-id="90cc2-147">Sedan kan du lagra hello röster för varje person i hello stad i hello-partition som motsvarar toothat stad.</span><span class="sxs-lookup"><span data-stu-id="90cc2-147">Then, you could store hello votes for every person in hello city in hello partition that corresponds toothat city.</span></span> <span data-ttu-id="90cc2-148">Bild 3 illustrerar en uppsättning personer och hello stad där de finns.</span><span class="sxs-lookup"><span data-stu-id="90cc2-148">Figure 3 illustrates a set of people and hello city in which they reside.</span></span>

![Enkel partition](./media/service-fabric-concepts-partitioning/cities.png)

<span data-ttu-id="90cc2-150">Eftersom hello ifyllning av städer varierar mycket, kan det sluta med några partitioner som innehåller stora mängder data (till exempel Seattle) och andra partitioner med mycket lite tillstånd (t.ex. Kirkland).</span><span class="sxs-lookup"><span data-stu-id="90cc2-150">As hello population of cities varies widely, you may end up with some partitions that contain a lot of data (e.g. Seattle) and other partitions with very little state (e.g. Kirkland).</span></span> <span data-ttu-id="90cc2-151">Vad är hello effekten av med partitioner med en ojämn mängder tillstånd?</span><span class="sxs-lookup"><span data-stu-id="90cc2-151">So what is hello impact of having partitions with uneven amounts of state?</span></span>

<span data-ttu-id="90cc2-152">Om du tycker om hello exempel igen kan kan du enkelt se att hello-partition som innehåller hello röster för Seattle kommer mer trafik än hello Kirkland en.</span><span class="sxs-lookup"><span data-stu-id="90cc2-152">If you think about hello example again, you can easily see that hello partition that holds hello votes for Seattle will get more traffic than hello Kirkland one.</span></span> <span data-ttu-id="90cc2-153">Som standard Service Fabric ser till att det handlar om hello samma antal primära och sekundära repliker på varje nod.</span><span class="sxs-lookup"><span data-stu-id="90cc2-153">By default, Service Fabric makes sure that there is about hello same number of primary and secondary replicas on each node.</span></span> <span data-ttu-id="90cc2-154">Därför kan det sluta med noder som har repliker som fungerar mer nätverkstrafik och andra som hanterar mindre trafik.</span><span class="sxs-lookup"><span data-stu-id="90cc2-154">So you may end up with nodes that hold replicas that serve more traffic and others that serve less traffic.</span></span> <span data-ttu-id="90cc2-155">Helst bör tooavoid hot och kalla platser som detta i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="90cc2-155">You would preferably want tooavoid hot and cold spots like this in a cluster.</span></span>

<span data-ttu-id="90cc2-156">I ordning tooavoid detta bör du göra två saker från en partitionering synsätt:</span><span class="sxs-lookup"><span data-stu-id="90cc2-156">In order tooavoid this, you should do two things, from a partitioning point of view:</span></span>

* <span data-ttu-id="90cc2-157">Försök toopartition hello tillstånd så att det är jämnt fördelad över alla partitioner.</span><span class="sxs-lookup"><span data-stu-id="90cc2-157">Try toopartition hello state so that it is evenly distributed across all partitions.</span></span>
* <span data-ttu-id="90cc2-158">Rapportera belastning från varje hello repliker för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="90cc2-158">Report load from each of hello replicas for hello service.</span></span> <span data-ttu-id="90cc2-159">(Mer information om hur checka ut den här artikeln på [mått och Läs in](service-fabric-cluster-resource-manager-metrics.md)).</span><span class="sxs-lookup"><span data-stu-id="90cc2-159">(For information on how, check out this article on [Metrics and Load](service-fabric-cluster-resource-manager-metrics.md)).</span></span> <span data-ttu-id="90cc2-160">Service Fabric ger hello kapaciteten tooreport belastning som används av tjänster, till exempel minne eller antal poster.</span><span class="sxs-lookup"><span data-stu-id="90cc2-160">Service Fabric provides hello capability tooreport load consumed by services, such as amount of memory or number of records.</span></span> <span data-ttu-id="90cc2-161">Baserat på hello mått rapporterade upptäcker Service Fabric att vissa partitioner hanterar högre belastning än andra och balanserar hello klustret genom att flytta repliker toomore lämplig noder, så att övergripande ingen nod är överbelastad.</span><span class="sxs-lookup"><span data-stu-id="90cc2-161">Based on hello metrics reported, Service Fabric detects that some partitions are serving higher loads than others and rebalances hello cluster by moving replicas toomore suitable nodes, so that overall no node is overloaded.</span></span>

<span data-ttu-id="90cc2-162">Du kan ibland vet hur mycket data visas i en given partition.</span><span class="sxs-lookup"><span data-stu-id="90cc2-162">Sometimes, you cannot know how much data will be in a given partition.</span></span> <span data-ttu-id="90cc2-163">Så att en allmän rekommendation är toodo båda--först sprids genom användning av en partitioneringsstrategi som hello data jämnt över hello partitioner och andra, med reporting belastningen.</span><span class="sxs-lookup"><span data-stu-id="90cc2-163">So a general recommendation is toodo both--first, by adopting a partitioning strategy that spreads hello data evenly across hello partitions and second, by reporting load.</span></span>  <span data-ttu-id="90cc2-164">hello första metoden förhindrar situationer som beskrivs i hello röstning exempelvis medan hello andra hjälper jämna ut temporära skillnader i åtkomst eller belastningsutjämning över tid.</span><span class="sxs-lookup"><span data-stu-id="90cc2-164">hello first method prevents situations described in hello voting example, while hello second helps smooth out temporary differences in access or load over time.</span></span>

<span data-ttu-id="90cc2-165">En annan aspekt av partition planering är toochoose hello rätt antal partitioner toobegin med.</span><span class="sxs-lookup"><span data-stu-id="90cc2-165">Another aspect of partition planning is toochoose hello correct number of partitions toobegin with.</span></span>
<span data-ttu-id="90cc2-166">Från ett Service Fabric-perspektiv finns det inget som förhindrar börjat med ett högre antal partitioner än förväntat för ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="90cc2-166">From a Service Fabric perspective, there is nothing that prevents you from starting out with a higher number of partitions than anticipated for your scenario.</span></span>
<span data-ttu-id="90cc2-167">I själva verket förutsatt att hello högsta tillåtna antalet partitioner är en giltig metod.</span><span class="sxs-lookup"><span data-stu-id="90cc2-167">In fact, assuming hello maximum number of partitions is a valid approach.</span></span>

<span data-ttu-id="90cc2-168">I sällsynta fall kan du få behöver fler partitioner än du ursprungligen har valt.</span><span class="sxs-lookup"><span data-stu-id="90cc2-168">In rare cases, you may end up needing more partitions than you have initially chosen.</span></span> <span data-ttu-id="90cc2-169">Du inte kan ändra antalet partitioner för hello efter hello fakta, behöver du tooapply vissa avancerade partition metoder, till exempel skapa en ny service-instans av hello samma typen tjänst.</span><span class="sxs-lookup"><span data-stu-id="90cc2-169">As you cannot change hello partition count after hello fact, you would need tooapply some advanced partition approaches, such as creating a new service instance of hello same service type.</span></span> <span data-ttu-id="90cc2-170">Du måste också tooimplement vissa klientsidan logik som dirigerar hello begär toohello rätt tjänstinstans, baserat på klientsidan kunskap som din klientkod måste underhåll.</span><span class="sxs-lookup"><span data-stu-id="90cc2-170">You would also need tooimplement some client-side logic that routes hello requests toohello correct service instance, based on client-side knowledge that your client code must maintain.</span></span>

<span data-ttu-id="90cc2-171">Ett annat övervägande för partitionering av planering är hello tillgängliga datorresurser.</span><span class="sxs-lookup"><span data-stu-id="90cc2-171">Another consideration for partitioning planning is hello available computer resources.</span></span> <span data-ttu-id="90cc2-172">Eftersom hello tillstånd måste toobe nås och lagras, är bundna toofollow:</span><span class="sxs-lookup"><span data-stu-id="90cc2-172">As hello state needs toobe accessed and stored, you are bound toofollow:</span></span>

* <span data-ttu-id="90cc2-173">Gränser för bandbredd i nätverket</span><span class="sxs-lookup"><span data-stu-id="90cc2-173">Network bandwidth limits</span></span>
* <span data-ttu-id="90cc2-174">System minnesgränserna</span><span class="sxs-lookup"><span data-stu-id="90cc2-174">System memory limits</span></span>
* <span data-ttu-id="90cc2-175">Disk Lagringsgränser</span><span class="sxs-lookup"><span data-stu-id="90cc2-175">Disk storage limits</span></span>

<span data-ttu-id="90cc2-176">Så vad händer om du stöter på resursen begränsningar i ett kluster som körs? hello svaret är att du kan helt enkelt skala ut hello tooaccommodate hello nya krav.</span><span class="sxs-lookup"><span data-stu-id="90cc2-176">So what happens if you run into resource constraints in a running cluster? hello answer is that you can simply scale out hello cluster tooaccommodate hello new requirements.</span></span>

<span data-ttu-id="90cc2-177">[Hej kapacitetsplaneringsguiden](service-fabric-capacity-planning.md) ger vägledning för hur toodetermine hur många noder i klustret måste.</span><span class="sxs-lookup"><span data-stu-id="90cc2-177">[hello capacity planning guide](service-fabric-capacity-planning.md) offers guidance for how toodetermine how many nodes your cluster needs.</span></span>

## <a name="get-started-with-partitioning"></a><span data-ttu-id="90cc2-178">Kom igång med partitionering</span><span class="sxs-lookup"><span data-stu-id="90cc2-178">Get started with partitioning</span></span>
<span data-ttu-id="90cc2-179">Det här avsnittet beskrivs hur tooget igång med partitionering din tjänst.</span><span class="sxs-lookup"><span data-stu-id="90cc2-179">This section describes how tooget started with partitioning your service.</span></span>

<span data-ttu-id="90cc2-180">Service Fabric kan välja mellan tre partitionsscheman:</span><span class="sxs-lookup"><span data-stu-id="90cc2-180">Service Fabric offers a choice of three partition schemes:</span></span>

* <span data-ttu-id="90cc2-181">Låg partitionering (kallas även UniformInt64Partition).</span><span class="sxs-lookup"><span data-stu-id="90cc2-181">Ranged partitioning (otherwise known as UniformInt64Partition).</span></span>
* <span data-ttu-id="90cc2-182">Namngivna partitionering.</span><span class="sxs-lookup"><span data-stu-id="90cc2-182">Named partitioning.</span></span> <span data-ttu-id="90cc2-183">Program med hjälp av den här modellen normalt har data som kan vara bucketed, inom en begränsad mängd.</span><span class="sxs-lookup"><span data-stu-id="90cc2-183">Applications using this model usually have data that can be bucketed, within a bounded set.</span></span> <span data-ttu-id="90cc2-184">Några vanliga exempel datafält som används som namngivna partitionsnycklar skulle vara regioner, postnummer, kundgrupper eller andra företag gränser.</span><span class="sxs-lookup"><span data-stu-id="90cc2-184">Some common examples of data fields used as named partition keys would be regions, postal codes, customer groups, or other business boundaries.</span></span>
* <span data-ttu-id="90cc2-185">Singleton partitioneras.</span><span class="sxs-lookup"><span data-stu-id="90cc2-185">Singleton partitioning.</span></span> <span data-ttu-id="90cc2-186">Singleton-partitioner används vanligtvis när hello-tjänsten inte kräver ytterligare routning.</span><span class="sxs-lookup"><span data-stu-id="90cc2-186">Singleton partitions are typically used when hello service does not require any additional routing.</span></span> <span data-ttu-id="90cc2-187">Till exempel använder tillståndslösa tjänster den här partitioneringsschema som standard.</span><span class="sxs-lookup"><span data-stu-id="90cc2-187">For example, stateless services use this partitioning scheme by default.</span></span>

<span data-ttu-id="90cc2-188">Namnet och scheman som Singleton partitionering är särskilda typer av låg partitioner.</span><span class="sxs-lookup"><span data-stu-id="90cc2-188">Named and Singleton partitioning schemes are special forms of ranged partitions.</span></span> <span data-ttu-id="90cc2-189">Som standard låg hello Visual Studio mallar för Service Fabric partitionering, eftersom den är hello vanligaste en.</span><span class="sxs-lookup"><span data-stu-id="90cc2-189">By default, hello Visual Studio templates for Service Fabric use ranged partitioning, as it is hello most common and useful one.</span></span> <span data-ttu-id="90cc2-190">hello resten av den här artikeln fokuserar på hello låg partitioneringsschema.</span><span class="sxs-lookup"><span data-stu-id="90cc2-190">hello remainder of this article focuses on hello ranged partitioning scheme.</span></span>

### <a name="ranged-partitioning-scheme"></a><span data-ttu-id="90cc2-191">Låg partitioneringsschema</span><span class="sxs-lookup"><span data-stu-id="90cc2-191">Ranged partitioning scheme</span></span>
<span data-ttu-id="90cc2-192">Detta är att använda toospecify heltal intervallet (som identifieras med en nyckel för låg och hög nyckel) och ett antal partitioner (n).</span><span class="sxs-lookup"><span data-stu-id="90cc2-192">This is used toospecify an integer range (identified by a low key and high key) and a number of partitions (n).</span></span> <span data-ttu-id="90cc2-193">Den skapar n partitioner, varje ansvarar för en icke-överlappande underintervall av hello övergripande partitions viktiga intervall.</span><span class="sxs-lookup"><span data-stu-id="90cc2-193">It creates n partitions, each responsible for a non-overlapping subrange of hello overall partition key range.</span></span> <span data-ttu-id="90cc2-194">Till exempel en ranged partitioneringsschema med en låg nyckel 0, övre nyckel 99 och antalet 4 skulle skapa fyra partitioner, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="90cc2-194">For example, a ranged partitioning scheme with a low key of 0, a high key of 99, and a count of 4 would create four partitions, as shown below.</span></span>

![Intervallet partitionering](./media/service-fabric-concepts-partitioning/range-partitioning.png)

<span data-ttu-id="90cc2-196">En vanlig metod är toocreate ett hash-värde baserat på en unik nyckel i hello datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="90cc2-196">A common approach is toocreate a hash based on a unique key within hello data set.</span></span> <span data-ttu-id="90cc2-197">Några vanliga exempel nycklar skulle vara vehicle ID-nummer (VIN), en anställnings-ID eller en unik sträng.</span><span class="sxs-lookup"><span data-stu-id="90cc2-197">Some common examples of keys would be a vehicle identification number (VIN), an employee ID, or a unique string.</span></span> <span data-ttu-id="90cc2-198">Med hjälp av den här Unik nyckel skulle du sedan skapa en hash-kod, modulus hello viktiga omfång, toouse som din nyckel.</span><span class="sxs-lookup"><span data-stu-id="90cc2-198">By using this unique key, you would then generate a hash code, modulus hello key range, toouse as your key.</span></span> <span data-ttu-id="90cc2-199">Du kan ange hello övre och nedre gränser för hello tillåtet viktiga intervall.</span><span class="sxs-lookup"><span data-stu-id="90cc2-199">You can specify hello upper and lower bounds of hello allowed key range.</span></span>

### <a name="select-a-hash-algorithm"></a><span data-ttu-id="90cc2-200">Välj en hash-algoritm</span><span class="sxs-lookup"><span data-stu-id="90cc2-200">Select a hash algorithm</span></span>
<span data-ttu-id="90cc2-201">En viktig del av hashing är att välja hash-algoritm.</span><span class="sxs-lookup"><span data-stu-id="90cc2-201">An important part of hashing is selecting your hash algorithm.</span></span> <span data-ttu-id="90cc2-202">En faktor är om hello målet är toogroup liknande nycklar nära varandra (ort känsliga hashing)-- eller om aktiviteten ska distribueras brett för alla partitioner (hash-distribution), vilket är mer vanligt.</span><span class="sxs-lookup"><span data-stu-id="90cc2-202">A consideration is whether hello goal is toogroup similar keys near each other (locality sensitive hashing)--or if activity should be distributed broadly across all partitions (distribution hashing), which is more common.</span></span>

<span data-ttu-id="90cc2-203">hello-egenskaperna hos en fungerande distribution hash-algoritm är att det är enkelt toocompute, den har några kollisioner och distribuerar hello nycklar jämnt.</span><span class="sxs-lookup"><span data-stu-id="90cc2-203">hello characteristics of a good distribution hashing algorithm are that it is easy toocompute, it has few collisions, and it distributes hello keys evenly.</span></span> <span data-ttu-id="90cc2-204">Ett bra exempel på en effektiv hash-algoritm är hello [FNV 1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) hash-algoritm.</span><span class="sxs-lookup"><span data-stu-id="90cc2-204">A good example of an efficient hash algorithm is hello [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) hash algorithm.</span></span>

<span data-ttu-id="90cc2-205">Mer allmän hash-kod algoritmen alternativ är hello [Wikipedia sida på hash-funktioner](http://en.wikipedia.org/wiki/Hash_function).</span><span class="sxs-lookup"><span data-stu-id="90cc2-205">A good resource for general hash code algorithm choices is hello [Wikipedia page on hash functions](http://en.wikipedia.org/wiki/Hash_function).</span></span>

## <a name="build-a-stateful-service-with-multiple-partitions"></a><span data-ttu-id="90cc2-206">Skapa en tillståndskänslig tjänst med flera partitioner</span><span class="sxs-lookup"><span data-stu-id="90cc2-206">Build a stateful service with multiple partitions</span></span>
<span data-ttu-id="90cc2-207">Nu ska vi skapa din första tillförlitliga tillståndskänslig tjänst med flera partitioner.</span><span class="sxs-lookup"><span data-stu-id="90cc2-207">Let's create your first reliable stateful service with multiple partitions.</span></span> <span data-ttu-id="90cc2-208">I det här exemplet skapar du ett enkelt program där du vill att toostore alla efternamn som börjar med samma enhetsbokstaven i hello hello samma partition.</span><span class="sxs-lookup"><span data-stu-id="90cc2-208">In this example, you will build a very simple application where you want toostore all last names that start with hello same letter in hello same partition.</span></span>

<span data-ttu-id="90cc2-209">Innan du skriva någon kod måste toothink om hello- och partitionsnycklar.</span><span class="sxs-lookup"><span data-stu-id="90cc2-209">Before you write any code, you need toothink about hello partitions and partition keys.</span></span> <span data-ttu-id="90cc2-210">Du behöver 26 partitioner (en för varje bokstav i alfabetet hello) men vad om hello låg och hög nycklar?</span><span class="sxs-lookup"><span data-stu-id="90cc2-210">You need 26 partitions (one for each letter in hello alphabet), but what about hello low and high keys?</span></span>
<span data-ttu-id="90cc2-211">Vi vill bokstavligt toohave en partition per brev, kan vi använder 0 som hello låg nyckel och 25 som hello hög nyckel, som varje bokstav är egen.</span><span class="sxs-lookup"><span data-stu-id="90cc2-211">As we literally want toohave one partition per letter, we can use 0 as hello low key and 25 as hello high key, as each letter is its own key.</span></span>

> [!NOTE]
> <span data-ttu-id="90cc2-212">Detta är en förenklad scenariot i verkligheten hello distribution är ojämnt.</span><span class="sxs-lookup"><span data-stu-id="90cc2-212">This is a simplified scenario, as in reality hello distribution would be uneven.</span></span> <span data-ttu-id="90cc2-213">Senaste namn som börjar med hello bokstäver ”S” eller ”M” är vanligare än hello som börjar med ”X” eller ”Y”.</span><span class="sxs-lookup"><span data-stu-id="90cc2-213">Last names starting with hello letters "S" or "M" are more common than hello ones starting with "X" or "Y".</span></span>
> 
> 

1. <span data-ttu-id="90cc2-214">Öppna **Visual Studio** > **filen** > **nya** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="90cc2-214">Open **Visual Studio** > **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="90cc2-215">I hello **nytt projekt** dialogrutan Välj hello Service Fabric-programmet.</span><span class="sxs-lookup"><span data-stu-id="90cc2-215">In hello **New Project** dialog box, choose hello Service Fabric application.</span></span>
3. <span data-ttu-id="90cc2-216">Anropa hello projektet ”AlphabetPartitions”.</span><span class="sxs-lookup"><span data-stu-id="90cc2-216">Call hello project "AlphabetPartitions".</span></span>
4. <span data-ttu-id="90cc2-217">I hello **skapar du en tjänst** dialogrutan Välj **Stateful** tjänst och anropa den ”Alphabet.Processing” enligt hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="90cc2-217">In hello **Create a Service** dialog box, choose **Stateful** service and call it "Alphabet.Processing" as shown in hello image below.</span></span>
       <span data-ttu-id="90cc2-218">![Dialogrutan Ny tjänst i Visual Studio][1]</span><span class="sxs-lookup"><span data-stu-id="90cc2-218">![New service dialog in Visual Studio][1]</span></span>

  <!--  ![Stateful service screenshot](./media/service-fabric-concepts-partitioning/createstateful.png)-->

5. <span data-ttu-id="90cc2-219">Ange hello antalet partitioner.</span><span class="sxs-lookup"><span data-stu-id="90cc2-219">Set hello number of partitions.</span></span> <span data-ttu-id="90cc2-220">Öppna hello Applicationmanifest.xml filen finns i hello ApplicationPackageRoot mapp för hello AlphabetPartitions projekt och uppdatera hello parametern Processing_PartitionCount too26 enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="90cc2-220">Open hello Applicationmanifest.xml file located in hello ApplicationPackageRoot folder of hello AlphabetPartitions project and update hello parameter Processing_PartitionCount too26 as shown below.</span></span>
   
    ```xml
    <Parameter Name="Processing_PartitionCount" DefaultValue="26" />
    ```
   
    <span data-ttu-id="90cc2-221">Du måste också tooupdate hello LowKey och HighKey egenskaper för hello StatefulService element i hello ApplicationManifest.xml enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="90cc2-221">You also need tooupdate hello LowKey and HighKey properties of hello StatefulService element in hello ApplicationManifest.xml as shown below.</span></span>
   
    ```xml
    <Service Name="Processing">
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="[Processing_TargetReplicaSetSize]" MinReplicaSetSize="[Processing_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Processing_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    ```
6. <span data-ttu-id="90cc2-222">För hello service toobe tillgänglig, öppna en slutpunkt på en port genom att lägga till hello endpoint element av ServiceManifest.xml (finns i hello PackageRoot mappen) för hello Alphabet.Processing service enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="90cc2-222">For hello service toobe accessible, open up an endpoint on a port by adding hello endpoint element of ServiceManifest.xml (located in hello PackageRoot folder) for hello Alphabet.Processing service as shown below:</span></span>
   
    ```xml
    <Endpoint Name="ProcessingServiceEndpoint" Port="8089" Protocol="http" Type="Internal" />
    ```
   
    <span data-ttu-id="90cc2-223">Hello-tjänsten är nu konfigurerad toolisten tooan intern slutpunkt med 26 partitioner.</span><span class="sxs-lookup"><span data-stu-id="90cc2-223">Now hello service is configured toolisten tooan internal endpoint with 26 partitions.</span></span>
7. <span data-ttu-id="90cc2-224">Sedan måste toooverride hello `CreateServiceReplicaListeners()` -metoden i klassen för hello-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="90cc2-224">Next, you need toooverride hello `CreateServiceReplicaListeners()` method of hello Processing class.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="90cc2-225">För det här exemplet förutsätter vi att du använder en enkel HttpCommunicationListener.</span><span class="sxs-lookup"><span data-stu-id="90cc2-225">For this sample, we assume that you are using a simple HttpCommunicationListener.</span></span> <span data-ttu-id="90cc2-226">Mer information om tillförlitlig kommunikation finns [hello tillförlitlig kommunikation tjänstmodell](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="90cc2-226">For more information on reliable service communication, see [hello Reliable Service communication model](service-fabric-reliable-services-communication.md).</span></span>
   > 
   > 
8. <span data-ttu-id="90cc2-227">En rekommenderad mönstret för hello URL: en som en replik som avlyssnar är hello följande format: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.</span><span class="sxs-lookup"><span data-stu-id="90cc2-227">A recommended pattern for hello URL that a replica listens on is hello following format: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.</span></span>
    <span data-ttu-id="90cc2-228">Så du tooconfigure din kommunikation lyssnare toolisten på hello rätt slutpunkter och med det här mönstret.</span><span class="sxs-lookup"><span data-stu-id="90cc2-228">So you want tooconfigure your communication listener toolisten on hello correct endpoints and with this pattern.</span></span>
   
    <span data-ttu-id="90cc2-229">Flera repliker av den här tjänsten kan finnas på hello samma dator, så den här adressen måste toobe unika toohello repliken.</span><span class="sxs-lookup"><span data-stu-id="90cc2-229">Multiple replicas of this service may be hosted on hello same computer, so this address needs toobe unique toohello replica.</span></span> <span data-ttu-id="90cc2-230">Det är därför partitions-ID + replik-ID är i hello-URL.</span><span class="sxs-lookup"><span data-stu-id="90cc2-230">This is why   partition ID + replica ID are in hello URL.</span></span> <span data-ttu-id="90cc2-231">HttpListener kan lyssna på flera adresser på hello samma port som hello URL-prefixet är unikt.</span><span class="sxs-lookup"><span data-stu-id="90cc2-231">HttpListener can listen on multiple addresses on hello same port as long as hello URL prefix    is unique.</span></span>
   
    <span data-ttu-id="90cc2-232">hello extra GUID finns det för ett avancerade fall där sekundära repliker också lyssna efter begäranden i skrivskyddat läge.</span><span class="sxs-lookup"><span data-stu-id="90cc2-232">hello extra GUID is there for an advanced case where secondary replicas also listen for read-only requests.</span></span> <span data-ttu-id="90cc2-233">När så är fallet hello vill toomake till att en ny unik adress används vid övergång från primära toosecondary tooforce klienter toore Lös hello adress.</span><span class="sxs-lookup"><span data-stu-id="90cc2-233">When that's hello case, you want toomake sure that a new unique address is used when transitioning from primary toosecondary tooforce clients toore-resolve hello address.</span></span> <span data-ttu-id="90cc2-234">”+” används som hello adress här så att hello repliken lyssnar på alla tillgängliga värdar (IP, FQDM, localhost, etc.) hello koden nedan visar ett exempel.</span><span class="sxs-lookup"><span data-stu-id="90cc2-234">'+' is used as hello address here so that hello replica listens on all available hosts (IP, FQDM, localhost, etc.) hello code below shows an example.</span></span>
   
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
   
    <span data-ttu-id="90cc2-235">Det är också värt att nämna att hello publicerade URL är något annorlunda än hello lyssnande URL-prefix.</span><span class="sxs-lookup"><span data-stu-id="90cc2-235">It's also worth noting that hello published URL is slightly different from hello listening URL prefix.</span></span>
    <span data-ttu-id="90cc2-236">hello lyssnande URL anges tooHttpListener.</span><span class="sxs-lookup"><span data-stu-id="90cc2-236">hello listening URL is given tooHttpListener.</span></span> <span data-ttu-id="90cc2-237">hello publicerade URL: en är hello-URL som är publicerade toohello Service Fabric Naming Service, som används för identifiering av tjänst.</span><span class="sxs-lookup"><span data-stu-id="90cc2-237">hello published URL is hello URL that is published toohello Service Fabric Naming Service, which is used for service discovery.</span></span> <span data-ttu-id="90cc2-238">Klienter begär den här adressen genom att discovery-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="90cc2-238">Clients will ask for this address through that discovery service.</span></span> <span data-ttu-id="90cc2-239">hello adress att klienter får behov toohave hello faktiska IP eller FQDN för hello nod i ordning tooconnect.</span><span class="sxs-lookup"><span data-stu-id="90cc2-239">hello address that clients get needs toohave hello actual IP or FQDN of hello node in order tooconnect.</span></span> <span data-ttu-id="90cc2-240">Så du måste tooreplace '+' med hello nodens IP eller FQDN som visas ovan.</span><span class="sxs-lookup"><span data-stu-id="90cc2-240">So you need tooreplace '+' with hello node's IP or FQDN as shown above.</span></span>
9. <span data-ttu-id="90cc2-241">hello sista steget är tooadd hello bearbetning logik toohello service enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="90cc2-241">hello last step is tooadd hello processing logic toohello service as shown below.</span></span>
   
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
   
    <span data-ttu-id="90cc2-242">`ProcessInternalRequest`läser hello värdena för hello frågan sträng parameter används toocall hello partition och anrop `AddUserAsync` tooadd hello efternamn toohello tillförlitliga ordlista `dictionary`.</span><span class="sxs-lookup"><span data-stu-id="90cc2-242">`ProcessInternalRequest` reads hello values of hello query string parameter used toocall hello partition and calls `AddUserAsync` tooadd hello lastname toohello reliable dictionary `dictionary`.</span></span>
10. <span data-ttu-id="90cc2-243">Lägg till en tillståndslösa tjänsten toohello projekt toosee hur du kan anropa en viss partition.</span><span class="sxs-lookup"><span data-stu-id="90cc2-243">Let's add a stateless service toohello project toosee how you can call a particular partition.</span></span>
    
    <span data-ttu-id="90cc2-244">Den här tjänsten fungerar som ett enkelt webbgränssnitt som accepterar hello efternamn som en frågesträngsparameter anger hello partitionsnyckel och skickar den toohello Alphabet.Processing service för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="90cc2-244">This service serves as a simple web interface that accepts hello lastname as a query string parameter, determines hello partition key, and sends it toohello Alphabet.Processing service for processing.</span></span>
11. <span data-ttu-id="90cc2-245">I hello **skapar du en tjänst** dialogrutan Välj **Stateless** tjänst och kalla den ”Alphabet.Web” enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="90cc2-245">In hello **Create a Service** dialog box, choose **Stateless** service and call it "Alphabet.Web" as shown below.</span></span>
    
    ![Skärmbild av tillståndslösa tjänsten](./media/service-fabric-concepts-partitioning/createnewstateless.png)<span data-ttu-id="90cc2-247">.</span><span class="sxs-lookup"><span data-stu-id="90cc2-247">.</span></span>
12. <span data-ttu-id="90cc2-248">Uppdatera hello endpoint informationen i hello ServiceManifest.xml hello Alphabet.WebApi service tooopen in en port som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="90cc2-248">Update hello endpoint information in hello ServiceManifest.xml of hello Alphabet.WebApi service tooopen up a port as shown below.</span></span>
    
    ```xml
    <Endpoint Name="WebApiServiceEndpoint" Protocol="http" Port="8081"/>
    ```
13. <span data-ttu-id="90cc2-249">Du måste tooreturn en mängd ServiceInstanceListeners i hello klass Web.</span><span class="sxs-lookup"><span data-stu-id="90cc2-249">You need tooreturn a collection of ServiceInstanceListeners in hello class Web.</span></span> <span data-ttu-id="90cc2-250">Igen, kan du välja tooimplement en enkel HttpCommunicationListener.</span><span class="sxs-lookup"><span data-stu-id="90cc2-250">Again, you can choose tooimplement a simple HttpCommunicationListener.</span></span>
    
    ```CSharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] {new ServiceInstanceListener(context => this.CreateInputListener(context))};
    }
    private ICommunicationListener CreateInputListener(ServiceContext context)
    {
        // Service instance's URL is hello node's IP & desired port
        EndpointResourceDescription inputEndpoint = context.CodePackageActivationContext.GetEndpoint("WebApiServiceEndpoint")
        string uriPrefix = String.Format("{0}://+:{1}/alphabetpartitions/", inputEndpoint.Protocol, inputEndpoint.Port);
        var uriPublished = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
        return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInputRequest);
    }
    ```
14. <span data-ttu-id="90cc2-251">Du måste nu tooimplement hello bearbetning logik.</span><span class="sxs-lookup"><span data-stu-id="90cc2-251">Now you need tooimplement hello processing logic.</span></span> <span data-ttu-id="90cc2-252">Hej HttpCommunicationListener anrop `ProcessInputRequest` när en begäran kommer.</span><span class="sxs-lookup"><span data-stu-id="90cc2-252">hello HttpCommunicationListener calls `ProcessInputRequest` when a request comes in.</span></span> <span data-ttu-id="90cc2-253">Så Låt oss och Lägg till hello koden nedan.</span><span class="sxs-lookup"><span data-stu-id="90cc2-253">So let's go ahead and add hello code below.</span></span>
    
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
                    "Result: {0}. <p>Partition key: '{1}' generated from hello first letter '{2}' of input value '{3}'. <br>Processing service partition ID: {4}. <br>Processing service replica address: {5}",
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
                output = output + "added tooPartition: " + primaryReplicaAddress;
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    ```
    
    <span data-ttu-id="90cc2-254">Nu ska vi gå igenom den steg för steg.</span><span class="sxs-lookup"><span data-stu-id="90cc2-254">Let's walk through it step by step.</span></span> <span data-ttu-id="90cc2-255">hello koden läser hello första bokstaven i hello frågesträngparametern `lastname` till char.</span><span class="sxs-lookup"><span data-stu-id="90cc2-255">hello code reads hello first letter of hello query string parameter `lastname` into a char.</span></span> <span data-ttu-id="90cc2-256">Sedan den avgör hello partitionsnyckel för den här bokstaven genom att subtrahera hello hexadecimalt värde på `A` från hello hexadecimalt värde för hello efternamn med versal.</span><span class="sxs-lookup"><span data-stu-id="90cc2-256">Then, it determines hello partition key for this letter by subtracting hello hexadecimal value of `A` from hello hexadecimal value of hello last names' first letter.</span></span>
    
    ```CSharp
    string lastname = context.Request.QueryString["lastname"];
    char firstLetterOfLastName = lastname.First();
    ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    ```
    
    <span data-ttu-id="90cc2-257">Kom ihåg att det här exemplet använder vi 26 partitioner med en partitionsnyckel per partition.</span><span class="sxs-lookup"><span data-stu-id="90cc2-257">Remember, for this example, we are using 26 partitions with one partition key per partition.</span></span>
    <span data-ttu-id="90cc2-258">Nu ska vi får hello service partition `partition` för den här nyckeln med hjälp av hello `ResolveAsync` metod på hello `servicePartitionResolver` objekt.</span><span class="sxs-lookup"><span data-stu-id="90cc2-258">Next, we obtain hello service partition `partition` for this key by using hello `ResolveAsync` method on hello `servicePartitionResolver` object.</span></span> <span data-ttu-id="90cc2-259">`servicePartitionResolver`definieras som</span><span class="sxs-lookup"><span data-stu-id="90cc2-259">`servicePartitionResolver` is defined as</span></span>
    
    ```CSharp
    private readonly ServicePartitionResolver servicePartitionResolver = ServicePartitionResolver.GetDefault();
    ```
    
    <span data-ttu-id="90cc2-260">Hej `ResolveAsync` hello partitionsnyckel, metoden tar hello tjänsten URI och en annullering token som parametrar.</span><span class="sxs-lookup"><span data-stu-id="90cc2-260">hello `ResolveAsync` method takes hello service URI, hello partition key, and a cancellation token as parameters.</span></span> <span data-ttu-id="90cc2-261">Hej URI för tjänsten för hello bearbetning av tjänsten är `fabric:/AlphabetPartitions/Processing`.</span><span class="sxs-lookup"><span data-stu-id="90cc2-261">hello service URI for hello processing service is `fabric:/AlphabetPartitions/Processing`.</span></span> <span data-ttu-id="90cc2-262">Kan gå vi hello slutpunkten för hello partition.</span><span class="sxs-lookup"><span data-stu-id="90cc2-262">Next, we get hello endpoint of hello partition.</span></span>
    
    ```CSharp
    ResolvedServiceEndpoint ep = partition.GetEndpoint()
    ```
    
    <span data-ttu-id="90cc2-263">Slutligen vi skapar hello slutpunkts-URL plus hello querystring och anropa hello bearbetning av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="90cc2-263">Finally, we build hello endpoint URL plus hello querystring and call hello processing service.</span></span>
    
    ```CSharp
    JObject addresses = JObject.Parse(ep.Address);
    string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
    UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
    primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
    string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    ```
    
    <span data-ttu-id="90cc2-264">När hello bearbetning görs skriva vi hello utdata tillbaka.</span><span class="sxs-lookup"><span data-stu-id="90cc2-264">Once hello processing is done, we write hello output back.</span></span>
15. <span data-ttu-id="90cc2-265">hello sista steget är tootest hello tjänst.</span><span class="sxs-lookup"><span data-stu-id="90cc2-265">hello last step is tootest hello service.</span></span> <span data-ttu-id="90cc2-266">Visual Studio använder programmet parametrar för lokal och distribution.</span><span class="sxs-lookup"><span data-stu-id="90cc2-266">Visual Studio uses application parameters for local and cloud deployment.</span></span> <span data-ttu-id="90cc2-267">tootest hello tjänsten lokalt 26 partitioner, behöver du tooupdate hello `Local.xml` filen i hello ApplicationParameters mappen för hello AlphabetPartitions projektet enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="90cc2-267">tootest hello service with 26 partitions locally, you need tooupdate hello `Local.xml` file in hello ApplicationParameters folder of hello AlphabetPartitions project as shown below:</span></span>
    
    ```xml
    <Parameters>
      <Parameter Name="Processing_PartitionCount" Value="26" />
      <Parameter Name="WebApi_InstanceCount" Value="1" />
    </Parameters>
    ```
16. <span data-ttu-id="90cc2-268">Du kan kontrollera hello-tjänsten och alla dess partitioner i hello Service Fabric Explorer när du är klar med distributionen.</span><span class="sxs-lookup"><span data-stu-id="90cc2-268">Once you finish deployment, you can check hello service and all of its partitions in hello Service Fabric Explorer.</span></span>
    
    ![Service Fabric Explorer skärmbild](./media/service-fabric-concepts-partitioning/sfxpartitions.png)
17. <span data-ttu-id="90cc2-270">I en webbläsare, kan du testa hello partitionering logik genom att ange `http://localhost:8081/?lastname=somename`.</span><span class="sxs-lookup"><span data-stu-id="90cc2-270">In a browser, you can test hello partitioning logic by entering `http://localhost:8081/?lastname=somename`.</span></span> <span data-ttu-id="90cc2-271">Du ser att varje efternamn som börjar med samma enhetsbeteckning som lagras i hello hello samma partition.</span><span class="sxs-lookup"><span data-stu-id="90cc2-271">You will see that each last name that starts with hello same letter is being stored in hello same partition.</span></span>
    
    ![Skärmbild för webbläsare](./media/service-fabric-concepts-partitioning/samplerunning.png)

<span data-ttu-id="90cc2-273">hello hela källkoden hello exemplet finns på [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span><span class="sxs-lookup"><span data-stu-id="90cc2-273">hello entire source code of hello sample is available on [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span></span>

## <a name="next-steps"></a><span data-ttu-id="90cc2-274">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="90cc2-274">Next steps</span></span>
<span data-ttu-id="90cc2-275">Information om Service Fabric-begrepp finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="90cc2-275">For information on Service Fabric concepts, see hello following:</span></span>

* [<span data-ttu-id="90cc2-276">Tillgänglighet för Service Fabric-tjänster</span><span class="sxs-lookup"><span data-stu-id="90cc2-276">Availability of Service Fabric services</span></span>](service-fabric-availability-services.md)
* [<span data-ttu-id="90cc2-277">Skalbarheten för Service Fabric-tjänster</span><span class="sxs-lookup"><span data-stu-id="90cc2-277">Scalability of Service Fabric services</span></span>](service-fabric-concepts-scalability.md)
* [<span data-ttu-id="90cc2-278">Kapacitetsplanering för Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="90cc2-278">Capacity planning for Service Fabric applications</span></span>](service-fabric-capacity-planning.md)

[wikipartition]: https://en.wikipedia.org/wiki/Partition_(database)

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png