---
title: "aaaAzure Service Fabric omvänd proxy | Microsoft Docs"
description: "Använda Service Fabric omvänd proxy för kommunikation toomicroservices från inom och utanför hello-kluster."
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/08/2017
ms.author: bharatn
ms.openlocfilehash: 0e7835a64ccd74293c7bdd8b41deae414c83dffa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reverse-proxy-in-azure-service-fabric"></a><span data-ttu-id="f8517-103">Omvänd proxy i Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f8517-103">Reverse proxy in Azure Service Fabric</span></span>
<span data-ttu-id="f8517-104">hello omvänd proxy som är inbyggd i Azure Service Fabric-adresser mikrotjänster i hello Service Fabric-kluster som exponerar HTTP-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="f8517-104">hello reverse proxy that's built into Azure Service Fabric addresses microservices in hello Service Fabric cluster that exposes HTTP endpoints.</span></span>

## <a name="microservices-communication-model"></a><span data-ttu-id="f8517-105">Mikrotjänster kommunikation modellen</span><span class="sxs-lookup"><span data-stu-id="f8517-105">Microservices communication model</span></span>
<span data-ttu-id="f8517-106">Mikrotjänster i Service Fabric normalt körs på en delmängd av virtuella datorer i hello klustret och kan flytta från en virtuell dator tooanother av olika skäl.</span><span class="sxs-lookup"><span data-stu-id="f8517-106">Microservices in Service Fabric typically run on a subset of virtual machines in hello cluster and can move from one virtual machine tooanother for various reasons.</span></span> <span data-ttu-id="f8517-107">Därför kan hello slutpunkter för mikrotjänster ändras dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="f8517-107">So, hello endpoints for microservices can change dynamically.</span></span> <span data-ttu-id="f8517-108">hello typiskt mönster toocommunicate toohello mikrotjänster är hello följande lösa slinga:</span><span class="sxs-lookup"><span data-stu-id="f8517-108">hello typical pattern toocommunicate toohello microservice is hello following resolve loop:</span></span>

1. <span data-ttu-id="f8517-109">Lös hello tjänstlokalisering ursprungligen via hello naming service.</span><span class="sxs-lookup"><span data-stu-id="f8517-109">Resolve hello service location initially through hello naming service.</span></span>
2. <span data-ttu-id="f8517-110">Ansluta toohello service.</span><span class="sxs-lookup"><span data-stu-id="f8517-110">Connect toohello service.</span></span>
3. <span data-ttu-id="f8517-111">Ta reda på hello orsaken för anslutningsfel och Lös hello tjänstlokalisering igen vid behov.</span><span class="sxs-lookup"><span data-stu-id="f8517-111">Determine hello cause of connection failures, and resolve hello service location again when necessary.</span></span>

<span data-ttu-id="f8517-112">Den här processen omfattar vanligtvis wrapping klientsidan kommunikations-bibliotek i en omförsöksslinga som implementerar hello-upplösning och försök igen principer.</span><span class="sxs-lookup"><span data-stu-id="f8517-112">This process generally involves wrapping client-side communication libraries in a retry loop that implements hello service resolution and retry policies.</span></span>
<span data-ttu-id="f8517-113">Mer information finns i [Connect och kommunicera med tjänster](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="f8517-113">For more information, see [Connect and communicate with services](service-fabric-connect-and-communicate-with-services.md).</span></span>

### <a name="communicating-by-using-hello-reverse-proxy"></a><span data-ttu-id="f8517-114">Kommunicerar med hjälp av hello omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="f8517-114">Communicating by using hello reverse proxy</span></span>
<span data-ttu-id="f8517-115">hello omvänd proxy i Service Fabric körs på alla hello-noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="f8517-115">hello reverse proxy in Service Fabric runs on all hello nodes in hello cluster.</span></span> <span data-ttu-id="f8517-116">Den utför hello hela tjänsten lösningsprocessen för klientens räkning och vidarebefordrar hello klientbegäran.</span><span class="sxs-lookup"><span data-stu-id="f8517-116">It performs hello entire service resolution process on a client's behalf and then forwards hello client request.</span></span> <span data-ttu-id="f8517-117">Klienter som körs på klustret hello kan så använder alla klientsidan http-kommunikation bibliotek tootalk toohello Måltjänsten med hjälp hello omvänd proxy att hello körs lokalt på samma nod.</span><span class="sxs-lookup"><span data-stu-id="f8517-117">So, clients that run on hello cluster can use any client-side HTTP communication libraries tootalk toohello target service by using hello reverse proxy that runs locally on hello same node.</span></span>

![Intern kommunikation][1]

## <a name="reaching-microservices-from-outside-hello-cluster"></a><span data-ttu-id="f8517-119">Nå mikrotjänster från utanför hello-kluster</span><span class="sxs-lookup"><span data-stu-id="f8517-119">Reaching microservices from outside hello cluster</span></span>
<span data-ttu-id="f8517-120">hello extern kommunikation standardmodell för mikrotjänster är en opt-in-modell där varje tjänst inte kan nås direkt från externa klienter.</span><span class="sxs-lookup"><span data-stu-id="f8517-120">hello default external communication model for microservices is an opt-in model where each service cannot be accessed directly from external clients.</span></span> <span data-ttu-id="f8517-121">[Azure belastningsutjämnare](../load-balancer/load-balancer-overview.md), vilket är en nätverksgräns mellan mikrotjänster och externa klienter utför nätverksadresser och vidarebefordrar externa begär toointernal IP:port slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="f8517-121">[Azure Load Balancer](../load-balancer/load-balancer-overview.md), which is a network boundary between microservices and external clients, performs network address translation and forwards external requests toointernal IP:port endpoints.</span></span> <span data-ttu-id="f8517-122">toomake en mikrotjänster endpoint direkt åtkomliga tooexternal klienter måste du först konfigurera belastningsutjämnaren tooforward trafik tooeach port som hello tjänsten använder i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="f8517-122">toomake a microservice's endpoint directly accessible tooexternal clients, you must first configure Load Balancer tooforward traffic tooeach port that hello service uses in hello cluster.</span></span> <span data-ttu-id="f8517-123">De flesta mikrotjänster, särskilt tillståndskänslig mikrotjänster Direktmigrering inte dessutom på alla noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="f8517-123">Furthermore, most microservices, especially stateful microservices, don't live on all nodes of hello cluster.</span></span> <span data-ttu-id="f8517-124">Hej mikrotjänster kan flytta mellan noder på redundanskluster.</span><span class="sxs-lookup"><span data-stu-id="f8517-124">hello microservices can move between nodes on failover.</span></span> <span data-ttu-id="f8517-125">I sådana fall belastningsutjämnaren effektivt kan inte fastställa hello plats för hello målnoden för hello repliker toowhich den ska vidarebefordra trafik.</span><span class="sxs-lookup"><span data-stu-id="f8517-125">In such cases, Load Balancer cannot effectively determine hello location of hello target node of hello replicas toowhich it should forward traffic.</span></span>

### <a name="reaching-microservices-via-hello-reverse-proxy-from-outside-hello-cluster"></a><span data-ttu-id="f8517-126">Nå mikrotjänster via hello omvänd proxy från utanför hello kluster</span><span class="sxs-lookup"><span data-stu-id="f8517-126">Reaching microservices via hello reverse proxy from outside hello cluster</span></span>
<span data-ttu-id="f8517-127">Du kan konfigurera precis hello port hello omvänd proxy i belastningsutjämnaren i stället för att konfigurera hello-port för en enskild tjänst i belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="f8517-127">Instead of configuring hello port of an individual service in Load Balancer, you can configure just hello port of hello reverse proxy in Load Balancer.</span></span> <span data-ttu-id="f8517-128">Den här konfigurationen kan klienter utanför hello kluster nå tjänster i hello kluster med hjälp av hello omvänd proxy utan ytterligare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f8517-128">This configuration lets clients outside hello cluster reach services inside hello cluster by using hello reverse proxy without additional configuration.</span></span>

![Extern kommunikation][0]

> [!WARNING]
> <span data-ttu-id="f8517-130">När du konfigurerar hello omvänd proxy-port i belastningsutjämnaren adresseras alla mikrotjänster i hello kluster som Exponerar en HTTP-slutpunkt från utanför hello kluster.</span><span class="sxs-lookup"><span data-stu-id="f8517-130">When you configure hello reverse proxy's port in Load Balancer, all microservices in hello cluster that expose an HTTP endpoint are addressable from outside hello cluster.</span></span>
>
>


## <a name="uri-format-for-addressing-services-by-using-hello-reverse-proxy"></a><span data-ttu-id="f8517-131">URI-format för adressering tjänster med hjälp av hello omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="f8517-131">URI format for addressing services by using hello reverse proxy</span></span>
<span data-ttu-id="f8517-132">hello omvänd proxy använder en specifik uniform resource identifier (URI) format tooidentify hello partition toowhich hello inkommande tjänstbegäran ska vidarebefordras:</span><span class="sxs-lookup"><span data-stu-id="f8517-132">hello reverse proxy uses a specific uniform resource identifier (URI) format tooidentify hello service partition toowhich hello incoming request should be forwarded:</span></span>

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&ListenerName=<listenerName>&TargetReplicaSelector=<targetReplicaSelector>&Timeout=<timeout_in_seconds>
```

* <span data-ttu-id="f8517-133">**http (s):** hello omvänd proxy kan vara konfigurerade tooaccept HTTP eller HTTPS-trafik.</span><span class="sxs-lookup"><span data-stu-id="f8517-133">**http(s):** hello reverse proxy can be configured tooaccept HTTP or HTTPS traffic.</span></span> <span data-ttu-id="f8517-134">HTTPS-vidarebefordran finns för[ansluta tooa säker service med hello omvänd proxy](service-fabric-reverseproxy-configure-secure-communication.md) när du har en omvänd proxy installationsprogrammet toolisten på HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f8517-134">For HTTPS forwarding, refer too[Connect tooa secure service with hello reverse proxy](service-fabric-reverseproxy-configure-secure-communication.md) once you have reverse proxy setup toolisten on HTTPS.</span></span>
* <span data-ttu-id="f8517-135">**Klustret fullständigt kvalificerade domännamnet (FQDN) | intern IP-adress:** för externa klienter kan du konfigurera hello omvänd proxy så att den kan nås via hello klustret domän, till exempel mycluster.eastus.cloudapp.azure.com. Som standard körs hello omvänd proxy på varje nod.</span><span class="sxs-lookup"><span data-stu-id="f8517-135">**Cluster fully qualified domain name (FQDN) | internal IP:** For external clients, you can configure hello reverse proxy so that it is reachable through hello cluster domain, such as mycluster.eastus.cloudapp.azure.com. By default, hello reverse proxy runs on every node.</span></span> <span data-ttu-id="f8517-136">För intern trafik kan hello omvänd proxy nås på localhost eller på alla interna noden IP-adresser, t.ex 10.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="f8517-136">For internal traffic, hello reverse proxy can be reached on localhost or on any internal node IP, such as 10.0.0.1.</span></span>
* <span data-ttu-id="f8517-137">**Port:** hello porten, till exempel 19081, som har angetts för hello omvänd proxy.</span><span class="sxs-lookup"><span data-stu-id="f8517-137">**Port:** This is hello port, such as 19081, that has been specified for hello reverse proxy.</span></span>
* <span data-ttu-id="f8517-138">**ServiceInstanceName:** är hello fullständigt kvalificerade namnet på hello distribuerat service-instans som du försöker tooreach utan hello ”fabric: /” schema.</span><span class="sxs-lookup"><span data-stu-id="f8517-138">**ServiceInstanceName:** This is hello fully-qualified name of hello deployed service instance that you are trying tooreach without hello "fabric:/" scheme.</span></span> <span data-ttu-id="f8517-139">Till exempel tooreach hello *fabric: / myapp/myservice/* tjänsten, som du vill använda *myapp/myservice*.</span><span class="sxs-lookup"><span data-stu-id="f8517-139">For example, tooreach hello *fabric:/myapp/myservice/* service, you would use *myapp/myservice*.</span></span>

    <span data-ttu-id="f8517-140">hello service-instansen är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="f8517-140">hello service instance name is case-sensitive.</span></span> <span data-ttu-id="f8517-141">Med hjälp av ett annat skiftläge för hello service instansnamn i hello URL orsakar hello begäranden toofail med 404 (inget hittas).</span><span class="sxs-lookup"><span data-stu-id="f8517-141">Using a different casing for hello service instance name in hello URL causes hello requests toofail with 404 (Not Found).</span></span>
* <span data-ttu-id="f8517-142">**Suffix sökväg:** detta är hello faktiska URL-sökväg som *myapi/värden/Lägg till/3*, för hello-tjänst som du vill tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="f8517-142">**Suffix path:** This is hello actual URL path, such as *myapi/values/add/3*, for hello service that you want tooconnect to.</span></span>
* <span data-ttu-id="f8517-143">**PartitionKey:** för en partitionerad tjänst är hello beräknade partitionsnyckel för hello-partition som du vill tooreach.</span><span class="sxs-lookup"><span data-stu-id="f8517-143">**PartitionKey:** For a partitioned service, this is hello computed partition key of hello partition that you want tooreach.</span></span> <span data-ttu-id="f8517-144">Observera att detta *inte* hello partitions-ID-GUID.</span><span class="sxs-lookup"><span data-stu-id="f8517-144">Note that this is *not* hello partition ID GUID.</span></span> <span data-ttu-id="f8517-145">Den här parametern krävs inte för tjänster som använder hello singleton-partitionsschema.</span><span class="sxs-lookup"><span data-stu-id="f8517-145">This parameter is not required for services that use hello singleton partition scheme.</span></span>
* <span data-ttu-id="f8517-146">**PartitionKind:** detta är hello partitionsschema för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f8517-146">**PartitionKind:** This is hello service partition scheme.</span></span> <span data-ttu-id="f8517-147">Detta kan vara 'Int64Range' eller 'Med namnet'.</span><span class="sxs-lookup"><span data-stu-id="f8517-147">This can be 'Int64Range' or 'Named'.</span></span> <span data-ttu-id="f8517-148">Den här parametern krävs inte för tjänster som använder hello singleton-partitionsschema.</span><span class="sxs-lookup"><span data-stu-id="f8517-148">This parameter is not required for services that use hello singleton partition scheme.</span></span>
* <span data-ttu-id="f8517-149">**ListenerName** hello slutpunkter från hello-tjänsten är hello formatet {”slutpunkter”: {”Listener1”: ”slutpunkt 1”, ”Listener2”: ”Endpoint2”...}}.</span><span class="sxs-lookup"><span data-stu-id="f8517-149">**ListenerName** hello endpoints from hello service are of hello form {"Endpoints":{"Listener1":"Endpoint1","Listener2":"Endpoint2" ...}}.</span></span> <span data-ttu-id="f8517-150">När hello-tjänsten visar flera slutpunkter, identifierar hello slutpunkt som hello klientbegäran ska vidarebefordras till.</span><span class="sxs-lookup"><span data-stu-id="f8517-150">When hello service exposes multiple endpoints, this identifies hello endpoint that hello client request should be forwarded to.</span></span> <span data-ttu-id="f8517-151">Detta kan utelämnas om hello-tjänsten har endast en lyssnare.</span><span class="sxs-lookup"><span data-stu-id="f8517-151">This can be omitted if hello service has only one listener.</span></span>
* <span data-ttu-id="f8517-152">**TargetReplicaSelector** anger hur hello replikuppsättningens eller instans måste väljas.</span><span class="sxs-lookup"><span data-stu-id="f8517-152">**TargetReplicaSelector** This specifies how hello target replica or instance should be selected.</span></span>
  * <span data-ttu-id="f8517-153">När hello Måltjänsten är tillståndskänslig hello TargetReplicaSelector kan vara något av följande hello: 'PrimaryReplica', 'RandomSecondaryReplica' eller 'RandomReplica'.</span><span class="sxs-lookup"><span data-stu-id="f8517-153">When hello target service is stateful, hello TargetReplicaSelector can be one of hello following:  'PrimaryReplica', 'RandomSecondaryReplica', or 'RandomReplica'.</span></span> <span data-ttu-id="f8517-154">Om den här parametern anges är hello standardvärdet 'PrimaryReplica'.</span><span class="sxs-lookup"><span data-stu-id="f8517-154">When this parameter is not specified, hello default is 'PrimaryReplica'.</span></span>
  * <span data-ttu-id="f8517-155">När hello Måltjänsten är tillståndslös hämtar en slumpmässig instans av hello partition tooforward hello tjänstbegäran för omvänd proxy.</span><span class="sxs-lookup"><span data-stu-id="f8517-155">When hello target service is stateless, reverse proxy picks a random instance of hello service partition tooforward hello request to.</span></span>
* <span data-ttu-id="f8517-156">**Timeout:** anger hello timeout för hello HTTP-begäran som skapats av hello omvänd proxy toohello tjänst på uppdrag av hello klientbegäran.</span><span class="sxs-lookup"><span data-stu-id="f8517-156">**Timeout:**  This specifies hello timeout for hello HTTP request created by hello reverse proxy toohello service on behalf of hello client request.</span></span> <span data-ttu-id="f8517-157">hello standardvärdet är 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="f8517-157">hello default value is 60 seconds.</span></span> <span data-ttu-id="f8517-158">Det här är en valfri parameter.</span><span class="sxs-lookup"><span data-stu-id="f8517-158">This is an optional parameter.</span></span>

### <a name="example-usage"></a><span data-ttu-id="f8517-159">Exempel på användning</span><span class="sxs-lookup"><span data-stu-id="f8517-159">Example usage</span></span>
<span data-ttu-id="f8517-160">Låt oss ta hello exempelvis *fabric: / MyApp/MyService* tjänst som öppnar en HTTP-lyssnare på hello följande URL:</span><span class="sxs-lookup"><span data-stu-id="f8517-160">As an example, let's take hello *fabric:/MyApp/MyService* service that opens an HTTP listener on hello following URL:</span></span>

```
http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

<span data-ttu-id="f8517-161">Följande är hello resurser för hello-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="f8517-161">Following are hello resources for hello service:</span></span>

* `/index.html`
* `/api/users/<userId>`

<span data-ttu-id="f8517-162">Om hello tjänsten använder hello singleton partitioneringsschema, hello *PartitionKey* och *PartitionKind* frågan string-parametrar är inte obligatoriska och hello-tjänsten kan nås med hjälp av hello-gateway som:</span><span class="sxs-lookup"><span data-stu-id="f8517-162">If hello service uses hello singleton partitioning scheme, hello *PartitionKey* and *PartitionKind* query string parameters are not required, and hello service can be reached by using hello gateway as:</span></span>

* <span data-ttu-id="f8517-163">Externt:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`</span><span class="sxs-lookup"><span data-stu-id="f8517-163">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`</span></span>
* <span data-ttu-id="f8517-164">Internt:`http://localhost:19081/MyApp/MyService`</span><span class="sxs-lookup"><span data-stu-id="f8517-164">Internally: `http://localhost:19081/MyApp/MyService`</span></span>

<span data-ttu-id="f8517-165">Om hello tjänsten använder hello Uniform Int64 partitioneringsschema, hello *PartitionKey* och *PartitionKind* frågan string-parametrar måste vara används tooreach en partition av hello-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="f8517-165">If hello service uses hello Uniform Int64 partitioning scheme, hello *PartitionKey* and *PartitionKind* query string parameters must be used tooreach a partition of hello service:</span></span>

* <span data-ttu-id="f8517-166">Externt:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="f8517-166">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span></span>
* <span data-ttu-id="f8517-167">Internt:`http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="f8517-167">Internally: `http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span></span>

<span data-ttu-id="f8517-168">tooreach hello resurser som hello-tjänsten visar Placera bara hello resursens sökväg efter hello tjänstnamnet i hello-URL:</span><span class="sxs-lookup"><span data-stu-id="f8517-168">tooreach hello resources that hello service exposes, simply place hello resource path after hello service name in hello URL:</span></span>

* <span data-ttu-id="f8517-169">Externt:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="f8517-169">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`</span></span>
* <span data-ttu-id="f8517-170">Internt:`http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="f8517-170">Internally: `http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`</span></span>

<span data-ttu-id="f8517-171">hello gateway kommer sedan att vidarebefordra dessa begäranden toohello tjänst-URL:</span><span class="sxs-lookup"><span data-stu-id="f8517-171">hello gateway will then forward these requests toohello service's URL:</span></span>

* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a><span data-ttu-id="f8517-172">Särskild hantering för delning av port tjänster</span><span class="sxs-lookup"><span data-stu-id="f8517-172">Special handling for port-sharing services</span></span>
<span data-ttu-id="f8517-173">Azure Application Gateway försöker tooresolve en tjänst adressen igen och försök hello begäran när en tjänst inte kan nås.</span><span class="sxs-lookup"><span data-stu-id="f8517-173">Azure Application Gateway attempts tooresolve a service address again and retry hello request when a service cannot be reached.</span></span> <span data-ttu-id="f8517-174">Detta är en större fördel av Programgateway eftersom klientkod inte behöver tooimplement sin egen service-upplösning och lösa loop.</span><span class="sxs-lookup"><span data-stu-id="f8517-174">This is a major benefit of Application Gateway because client code does not need tooimplement its own service resolution and resolve loop.</span></span>

<span data-ttu-id="f8517-175">I allmänhet när en tjänst inte kan nås, har hello tjänstinstansen eller replik flyttats tooa annan nod som en del av sin normala livscykel.</span><span class="sxs-lookup"><span data-stu-id="f8517-175">Generally, when a service cannot be reached, hello service instance or replica has moved tooa different node as part of its normal lifecycle.</span></span> <span data-ttu-id="f8517-176">När detta sker få Programgateway ett nätverk anslutning fel som anger att en slutpunkt är inte längre öppna på hello ursprungligen matcha adress.</span><span class="sxs-lookup"><span data-stu-id="f8517-176">When this happens, Application Gateway might receive a network connection error indicating that an endpoint is no longer open on hello originally resolved address.</span></span>

<span data-ttu-id="f8517-177">Dock replikeringar eller instanser av tjänsten kan dela en värdprocess och kan också dela en port när finns en http.sys-baserade webbservern, inklusive:</span><span class="sxs-lookup"><span data-stu-id="f8517-177">However, replicas or service instances can share a host process and might also share a port when hosted by an http.sys-based web server, including:</span></span>

* [<span data-ttu-id="f8517-178">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="f8517-178">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
* [<span data-ttu-id="f8517-179">ASP.NET Core WebListener</span><span class="sxs-lookup"><span data-stu-id="f8517-179">ASP.NET Core WebListener</span></span>](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
* [<span data-ttu-id="f8517-180">Katana</span><span class="sxs-lookup"><span data-stu-id="f8517-180">Katana</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

<span data-ttu-id="f8517-181">I det här fallet är det troligt webbservern hello finns i hello värdprocess och svarar toorequests, men hello löst tjänstinstansen eller repliken är inte längre tillgänglig på hello värden.</span><span class="sxs-lookup"><span data-stu-id="f8517-181">In this situation, it is likely that hello web server is available in hello host process and responding toorequests, but hello resolved service instance or replica is no longer available on hello host.</span></span> <span data-ttu-id="f8517-182">I det här fallet får hello gateway ett HTTP 404-svar från hello webbservern.</span><span class="sxs-lookup"><span data-stu-id="f8517-182">In this case, hello gateway will receive an HTTP 404 response from hello web server.</span></span> <span data-ttu-id="f8517-183">Därför har en HTTP 404 två distinkta innebörd:</span><span class="sxs-lookup"><span data-stu-id="f8517-183">Thus, an HTTP 404 has two distinct meanings:</span></span>

- <span data-ttu-id="f8517-184">Fall #1: hello-adress är korrekt, men hello-resurs som hello begärd användare finns inte.</span><span class="sxs-lookup"><span data-stu-id="f8517-184">Case #1: hello service address is correct, but hello resource that hello user requested does not exist.</span></span>
- <span data-ttu-id="f8517-185">Fall #2: hello-adress är felaktig och hello-resurs som hello begärd användare kan finnas på en annan nod.</span><span class="sxs-lookup"><span data-stu-id="f8517-185">Case #2: hello service address is incorrect, and hello resource that hello user requested might exist on a different node.</span></span>

<span data-ttu-id="f8517-186">hello första fall har en normal HTTP 404 som anses vara ett användarfel.</span><span class="sxs-lookup"><span data-stu-id="f8517-186">hello first case is a normal HTTP 404, which is considered a user error.</span></span> <span data-ttu-id="f8517-187">I andra fall hello har dock hello användaren begärde en resurs som finns.</span><span class="sxs-lookup"><span data-stu-id="f8517-187">However, in hello second case, hello user has requested a resource that does exist.</span></span> <span data-ttu-id="f8517-188">Programgateway kunde toolocate den eftersom hello-tjänsten har flyttats.</span><span class="sxs-lookup"><span data-stu-id="f8517-188">Application Gateway was unable toolocate it because hello service itself has moved.</span></span> <span data-ttu-id="f8517-189">Programmet måste tooresolve hello gatewayadress igen och försök igen hello begäran.</span><span class="sxs-lookup"><span data-stu-id="f8517-189">Application Gateway needs tooresolve hello address again and retry hello request.</span></span>

<span data-ttu-id="f8517-190">Programgateway innebär behöver ett sätt toodistinguish mellan dessa två fall.</span><span class="sxs-lookup"><span data-stu-id="f8517-190">Application Gateway thus needs a way toodistinguish between these two cases.</span></span> <span data-ttu-id="f8517-191">toomake att distinktion en ledtråd från hello-server krävs.</span><span class="sxs-lookup"><span data-stu-id="f8517-191">toomake that distinction, a hint from hello server is required.</span></span>

* <span data-ttu-id="f8517-192">Som standard Programgateway förutsätter fall #2 och försöker tooresolve och utfärda hello begäran igen.</span><span class="sxs-lookup"><span data-stu-id="f8517-192">By default, Application Gateway assumes case #2 and attempts tooresolve and issue hello request again.</span></span>
* <span data-ttu-id="f8517-193">tooindicate fall #1 tooApplication Gateway hello-tjänsten ska returnera följande HTTP-Svarsrubrik hello:</span><span class="sxs-lookup"><span data-stu-id="f8517-193">tooindicate case #1 tooApplication Gateway, hello service should return hello following HTTP response header:</span></span>

  `X-ServiceFabric : ResourceNotFound`

<span data-ttu-id="f8517-194">Den här HTTP-Svarsrubrik visar en normal HTTP 404-situation i vilken hello begärda resursen inte finns och Programgateway försöker inte tooresolve hello-adress igen.</span><span class="sxs-lookup"><span data-stu-id="f8517-194">This HTTP response header indicates a normal HTTP 404 situation in which hello requested resource does not exist, and Application Gateway will not attempt tooresolve hello service address again.</span></span>

## <a name="setup-and-configuration"></a><span data-ttu-id="f8517-195">Installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="f8517-195">Setup and configuration</span></span>

### <a name="enable-reverse-proxy-via-azure-portal"></a><span data-ttu-id="f8517-196">Aktivera omvänd proxy via Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f8517-196">Enable reverse proxy via Azure portal</span></span>

<span data-ttu-id="f8517-197">Azure-portalen innehåller en omvänd proxy för alternativet-tooenable när du skapar ett nytt Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="f8517-197">Azure portal provides an option tooenable Reverse proxy while creating a new Service Fabric cluster.</span></span>
<span data-ttu-id="f8517-198">Under **skapar Service Fabric-kluster**, steg 2: klusterkonfiguration, konfiguration av noden typ, markera kryssrutan för hello för ”aktivera omvänd proxy”.</span><span class="sxs-lookup"><span data-stu-id="f8517-198">Under **Create Service Fabric cluster**, Step 2: Cluster Configuration, Node type configuration, select hello checkbox too"Enable reverse proxy".</span></span>
<span data-ttu-id="f8517-199">För att konfigurera säker omvänd proxy, SSL-certifikat kan anges i steg3: säkerhet, konfigurera säkerhetsinställningar för klustret, väljer hello kryssrutan för ”innehåller ett SSL-certifikat för omvänd proxy” och ange hello-certifikatinformation.</span><span class="sxs-lookup"><span data-stu-id="f8517-199">For configuring secure reverse proxy, SSL certificate can be specified in Step 3: Security, Configure cluster security settings, select hello checkbox too"Include a SSL certificate for reverse proxy" and enter hello certificate details.</span></span>

### <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a><span data-ttu-id="f8517-200">Aktivera omvänd proxy via Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="f8517-200">Enable reverse proxy via Azure Resource Manager templates</span></span>

<span data-ttu-id="f8517-201">Du kan använda hello [Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md) tooenable hello omvänd proxy i Service Fabric för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="f8517-201">You can use hello [Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md) tooenable hello reverse proxy in Service Fabric for hello cluster.</span></span>

<span data-ttu-id="f8517-202">Se för[konfigurera HTTPS omvänd Proxy i ett kluster för säker](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) för Azure Resource Manager mallen exempel tooconfigure säker omvänd proxy med förnya ett certifikat och hantering av certifikatet.</span><span class="sxs-lookup"><span data-stu-id="f8517-202">Refer too[Configure HTTPS Reverse Proxy in a secure cluster](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) for Azure Resource Manager template samples tooconfigure secure reverse proxy with a certificate and handling certificate rollover.</span></span>

<span data-ttu-id="f8517-203">Först får du hello mall för hello klustret som du vill toodeploy.</span><span class="sxs-lookup"><span data-stu-id="f8517-203">First, you get hello template for hello cluster that you want toodeploy.</span></span> <span data-ttu-id="f8517-204">Du kan använda hello exempelmallarna, eller så kan du skapa en anpassad mall för hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="f8517-204">You can either use hello sample templates or create a custom Resource Manager template.</span></span> <span data-ttu-id="f8517-205">Sedan kan du aktivera hello omvänd proxy genom att använda hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f8517-205">Then, you can enable hello reverse proxy by using hello following steps:</span></span>

1. <span data-ttu-id="f8517-206">Definiera en port för hello omvänd proxy i hello [parametrar avsnittet](../azure-resource-manager/resource-group-authoring-templates.md) för hello mall.</span><span class="sxs-lookup"><span data-stu-id="f8517-206">Define a port for hello reverse proxy in hello [Parameters section](../azure-resource-manager/resource-group-authoring-templates.md) of hello template.</span></span>

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. <span data-ttu-id="f8517-207">Ange hello-port för varje hello nodetype objekt i hello **klustret** [typen avsnittet](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f8517-207">Specify hello port for each of hello nodetype objects in hello **Cluster** [Resource type section](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

    <span data-ttu-id="f8517-208">hello port identifieras av hello parameternamn, reverseProxyEndpointPort.</span><span class="sxs-lookup"><span data-stu-id="f8517-208">hello port is identified by hello parameter name, reverseProxyEndpointPort.</span></span>

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```
3. <span data-ttu-id="f8517-209">tooaddress hello omvänd proxy från utanför hello Azure kluster, ange hello Azure belastningsutjämnare regler för hello-port som du angav i steg 1.</span><span class="sxs-lookup"><span data-stu-id="f8517-209">tooaddress hello reverse proxy from outside hello Azure cluster, set up hello Azure Load Balancer rules for hello port that you specified in step 1.</span></span>

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. <span data-ttu-id="f8517-210">tooconfigure SSL-certifikat på hello port hello omvänd proxy, lägga till hello certifikat toohello ***reverseProxyCertificate*** egenskap i hello **klustret** [typen avsnittet](../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f8517-210">tooconfigure SSL certificates on hello port for hello reverse proxy, add hello certificate toohello ***reverseProxyCertificate*** property in hello **Cluster** [Resource type section](../resource-group-authoring-templates.md).</span></span>

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-hello-cluster-certificate"></a><span data-ttu-id="f8517-211">Stöd för en omvänd proxy-certifikat som skiljer sig från hello klustret certifikatet</span><span class="sxs-lookup"><span data-stu-id="f8517-211">Supporting a reverse proxy certificate that's different from hello cluster certificate</span></span>
 <span data-ttu-id="f8517-212">Om hello omvänd proxycertifikatet skiljer sig från hello-certifikat som skyddar hello klustret sedan angiven hello tidigare certifikatet ska installeras på den virtuella datorn hello och lagt till toohello åtkomstkontrollistan (ACL) så att Service Fabric kan komma åt den.</span><span class="sxs-lookup"><span data-stu-id="f8517-212">If hello reverse proxy certificate is different from hello certificate that secures hello cluster, then hello previously specified certificate should be installed on hello virtual machine and added toohello access control list (ACL) so that Service Fabric can access it.</span></span> <span data-ttu-id="f8517-213">Detta kan göras i hello **virtualMachineScaleSets** [typen avsnittet](../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f8517-213">This can be done in hello **virtualMachineScaleSets** [Resource type section](../resource-group-authoring-templates.md).</span></span> <span data-ttu-id="f8517-214">Lägg till att certifikatet toohello osProfile för installation.</span><span class="sxs-lookup"><span data-stu-id="f8517-214">For installation, add that certificate toohello osProfile.</span></span> <span data-ttu-id="f8517-215">hello tillägget avsnitt i hello mall kan uppdatera hello certifikat i hello ACL.</span><span class="sxs-lookup"><span data-stu-id="f8517-215">hello extension section of hello template can update hello certificate in hello ACL.</span></span>

  ```json
  {
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    ....
      "osProfile": {
          "adminPassword": "[parameters('adminPassword')]",
          "adminUsername": "[parameters('adminUsername')]",
          "computernamePrefix": "[parameters('vmNodeType0Name')]",
          "secrets": [
            {
              "sourceVault": {
                "id": "[parameters('sfReverseProxySourceVaultValue')]"
              },
              "vaultCertificates": [
                {
                  "certificateStore": "[parameters('sfReverseProxyCertificateStoreValue')]",
                  "certificateUrl": "[parameters('sfReverseProxyCertificateUrlValue')]"
                }
              ]
            }
          ]
        }
   ....
   "extensions": [
          {
              "name": "[concat(parameters('vmNodeType0Name'),'_ServiceFabricNode')]",
              "properties": {
                      "type": "ServiceFabricNode",
                      "autoUpgradeMinorVersion": false,
                      ...
                      "publisher": "Microsoft.Azure.ServiceFabric",
                      "settings": {
                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                        "dataPath": "D:\\\\SvcFab",
                        "durabilityLevel": "Bronze",
                        "testExtension": true,
                        "reverseProxyCertificate": {
                          "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                          "x509StoreName": "[parameters('sfReverseProxyCertificateStoreValue')]"
                        },
                  },
                  "typeHandlerVersion": "1.0"
              }
          },
      ]
    }
  ```
> [!NOTE]
> <span data-ttu-id="f8517-216">När du använder certifikat som skiljer sig från hello klustret certifikat tooenable hello omvänd proxy på ett befintligt kluster, installera hello omvänd proxycertifikatet och uppdatera hello ACL på hello klustret innan du aktiverar hello omvänd proxy.</span><span class="sxs-lookup"><span data-stu-id="f8517-216">When you use certificates that are different from hello cluster certificate tooenable hello reverse proxy on an existing cluster, install hello reverse proxy certificate and update hello ACL on hello cluster before you enable hello reverse proxy.</span></span> <span data-ttu-id="f8517-217">Fullständig hello [Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md) distribution genom att använda hello-inställningar som anges tidigare innan du startar en omvänd proxy för distribution tooenable hello i steg 1 – 4.</span><span class="sxs-lookup"><span data-stu-id="f8517-217">Complete hello [Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md) deployment by using hello settings mentioned previously before you start a deployment tooenable hello reverse proxy in steps 1-4.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8517-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f8517-218">Next steps</span></span>
* <span data-ttu-id="f8517-219">Se ett exempel på HTTP-kommunikation mellan tjänster i en [exempelprojektet på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="f8517-219">See an example of HTTP communication between services in a [sample project on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span></span>
* [<span data-ttu-id="f8517-220">Vidarebefordran toosecure HTTP-tjänsten med hello omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="f8517-220">Forwarding toosecure HTTP service with hello reverse proxy</span></span>](service-fabric-reverseproxy-configure-secure-communication.md)
* [<span data-ttu-id="f8517-221">RPC-anrop med Reliable Services fjärrkommunikation</span><span class="sxs-lookup"><span data-stu-id="f8517-221">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="f8517-222">Webb-API som använder OWIN i Reliable Services</span><span class="sxs-lookup"><span data-stu-id="f8517-222">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="f8517-223">WCF-kommunikation med hjälp av Reliable Services</span><span class="sxs-lookup"><span data-stu-id="f8517-223">WCF communication by using Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
* <span data-ttu-id="f8517-224">Ytterligare omvänd proxy konfigurationsalternativ finns ApplicationGateway/http-avsnittet i [anpassa Service Fabric-klusterinställningar](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="f8517-224">For additional reverse proxy configuration options, refer ApplicationGateway/Http section in [Customize Service Fabric cluster settings](service-fabric-cluster-fabric-settings.md).</span></span>

[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
