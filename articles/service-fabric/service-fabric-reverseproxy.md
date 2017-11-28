---
title: "Azure Service Fabric omvänd proxy | Microsoft Docs"
description: "Använda Service Fabric omvänd proxy för kommunikation med mikrotjänster från inom och utanför klustret."
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
ms.openlocfilehash: 7897458e9e4a0bbe185bd3f7b4c133c1b26769f9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="reverse-proxy-in-azure-service-fabric"></a><span data-ttu-id="1b0ce-103">Omvänd proxy i Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1b0ce-103">Reverse proxy in Azure Service Fabric</span></span>
<span data-ttu-id="1b0ce-104">Omvänd proxy som är inbyggd i Azure Service Fabric-adresser mikrotjänster i Service Fabric-klustret som exponerar HTTP-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-104">The reverse proxy that's built into Azure Service Fabric addresses microservices in the Service Fabric cluster that exposes HTTP endpoints.</span></span>

## <a name="microservices-communication-model"></a><span data-ttu-id="1b0ce-105">Mikrotjänster kommunikation modellen</span><span class="sxs-lookup"><span data-stu-id="1b0ce-105">Microservices communication model</span></span>
<span data-ttu-id="1b0ce-106">Mikrotjänster i Service Fabric normalt körs på en delmängd av virtuella datorer i klustret och kan flytta från en virtuell dator till en annan av olika skäl.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-106">Microservices in Service Fabric typically run on a subset of virtual machines in the cluster and can move from one virtual machine to another for various reasons.</span></span> <span data-ttu-id="1b0ce-107">Därför kan slutpunkterna för mikrotjänster ändras dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-107">So, the endpoints for microservices can change dynamically.</span></span> <span data-ttu-id="1b0ce-108">Typiskt mönster för att kommunicera med mikrotjänster är följande Lös slinga:</span><span class="sxs-lookup"><span data-stu-id="1b0ce-108">The typical pattern to communicate to the microservice is the following resolve loop:</span></span>

1. <span data-ttu-id="1b0ce-109">Lös tjänstlokalisering ursprungligen via naming service.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-109">Resolve the service location initially through the naming service.</span></span>
2. <span data-ttu-id="1b0ce-110">Ansluta till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-110">Connect to the service.</span></span>
3. <span data-ttu-id="1b0ce-111">Ta reda på orsaken för anslutningsfel och Lös service platsen igen vid behov.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-111">Determine the cause of connection failures, and resolve the service location again when necessary.</span></span>

<span data-ttu-id="1b0ce-112">Den här processen omfattar vanligtvis wrapping klientsidan kommunikations-bibliotek i en omförsöksslinga som implementerar serviceprinciper upplösning och försök igen.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-112">This process generally involves wrapping client-side communication libraries in a retry loop that implements the service resolution and retry policies.</span></span>
<span data-ttu-id="1b0ce-113">Mer information finns i [Connect och kommunicera med tjänster](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="1b0ce-113">For more information, see [Connect and communicate with services](service-fabric-connect-and-communicate-with-services.md).</span></span>

### <a name="communicating-by-using-the-reverse-proxy"></a><span data-ttu-id="1b0ce-114">Kommunicerar med hjälp av omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="1b0ce-114">Communicating by using the reverse proxy</span></span>
<span data-ttu-id="1b0ce-115">Omvänd proxy i Service Fabric körs på alla noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-115">The reverse proxy in Service Fabric runs on all the nodes in the cluster.</span></span> <span data-ttu-id="1b0ce-116">Den utför hela tjänsten lösningsprocessen för klientens räkning och vidarebefordrar klientbegäran.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-116">It performs the entire service resolution process on a client's behalf and then forwards the client request.</span></span> <span data-ttu-id="1b0ce-117">Klienter som körs på klustret kan därför använda alla klientsidan http-kommunikation bibliotek tala med Måltjänsten via omvänd proxy som körs lokalt på samma nod.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-117">So, clients that run on the cluster can use any client-side HTTP communication libraries to talk to the target service by using the reverse proxy that runs locally on the same node.</span></span>

![Intern kommunikation][1]

## <a name="reaching-microservices-from-outside-the-cluster"></a><span data-ttu-id="1b0ce-119">Nå mikrotjänster från utanför klustret</span><span class="sxs-lookup"><span data-stu-id="1b0ce-119">Reaching microservices from outside the cluster</span></span>
<span data-ttu-id="1b0ce-120">Standardmodell för extern kommunikation för mikrotjänster är en opt-in-modell där varje tjänst inte kan nås direkt från externa klienter.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-120">The default external communication model for microservices is an opt-in model where each service cannot be accessed directly from external clients.</span></span> <span data-ttu-id="1b0ce-121">[Azure belastningsutjämnare](../load-balancer/load-balancer-overview.md), vilket är en nätverksgräns mellan mikrotjänster och externa klienter utför nätverksadresser och vidarebefordrar externa begäranden till interna IP:port slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-121">[Azure Load Balancer](../load-balancer/load-balancer-overview.md), which is a network boundary between microservices and external clients, performs network address translation and forwards external requests to internal IP:port endpoints.</span></span> <span data-ttu-id="1b0ce-122">Om du vill göra en mikrotjänster endpoint direkt tillgänglig för externa klienter, måste du först konfigurera belastningsfördelning, så att vidarebefordra trafik till varje port som tjänsten använder i klustret.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-122">To make a microservice's endpoint directly accessible to external clients, you must first configure Load Balancer to forward traffic to each port that the service uses in the cluster.</span></span> <span data-ttu-id="1b0ce-123">De flesta mikrotjänster, särskilt tillståndskänslig mikrotjänster Direktmigrering inte dessutom på alla noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-123">Furthermore, most microservices, especially stateful microservices, don't live on all nodes of the cluster.</span></span> <span data-ttu-id="1b0ce-124">Mikrotjänster kan flytta mellan noder på redundanskluster.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-124">The microservices can move between nodes on failover.</span></span> <span data-ttu-id="1b0ce-125">I sådana fall kan inte belastningsutjämnaren effektivt fastställa platsen för målnoden repliker som den ska vidarebefordra trafik.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-125">In such cases, Load Balancer cannot effectively determine the location of the target node of the replicas to which it should forward traffic.</span></span>

### <a name="reaching-microservices-via-the-reverse-proxy-from-outside-the-cluster"></a><span data-ttu-id="1b0ce-126">Nå mikrotjänster via omvänd proxy från utanför klustret</span><span class="sxs-lookup"><span data-stu-id="1b0ce-126">Reaching microservices via the reverse proxy from outside the cluster</span></span>
<span data-ttu-id="1b0ce-127">Du kan konfigurera precis porten för omvänd proxy i belastningsutjämnaren i stället för att konfigurera porten för en enskild tjänst i belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-127">Instead of configuring the port of an individual service in Load Balancer, you can configure just the port of the reverse proxy in Load Balancer.</span></span> <span data-ttu-id="1b0ce-128">Den här konfigurationen kan klienter utanför klustret nå tjänster i klustret via omvänd proxy utan ytterligare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-128">This configuration lets clients outside the cluster reach services inside the cluster by using the reverse proxy without additional configuration.</span></span>

![Extern kommunikation][0]

> [!WARNING]
> <span data-ttu-id="1b0ce-130">När du konfigurerar omvänd proxy-port i belastningsutjämnaren adresseras alla mikrotjänster i klustret som Exponerar en HTTP-slutpunkt från utanför klustret.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-130">When you configure the reverse proxy's port in Load Balancer, all microservices in the cluster that expose an HTTP endpoint are addressable from outside the cluster.</span></span>
>
>


## <a name="uri-format-for-addressing-services-by-using-the-reverse-proxy"></a><span data-ttu-id="1b0ce-131">URI-format för adressering tjänster med hjälp av omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="1b0ce-131">URI format for addressing services by using the reverse proxy</span></span>
<span data-ttu-id="1b0ce-132">Omvänd proxy använder formatet specifika uniform resource identifier (URI) för att identifiera tjänsten partitionen som den inkommande begäranden ska vidarebefordras:</span><span class="sxs-lookup"><span data-stu-id="1b0ce-132">The reverse proxy uses a specific uniform resource identifier (URI) format to identify the service partition to which the incoming request should be forwarded:</span></span>

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&ListenerName=<listenerName>&TargetReplicaSelector=<targetReplicaSelector>&Timeout=<timeout_in_seconds>
```

* <span data-ttu-id="1b0ce-133">**http (s):** omvänd proxy kan konfigureras för att godkänna HTTP eller HTTPS-trafik.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-133">**http(s):** The reverse proxy can be configured to accept HTTP or HTTPS traffic.</span></span> <span data-ttu-id="1b0ce-134">HTTPS-vidarebefordran finns [Anslut till en säker tjänst med omvänd proxy](service-fabric-reverseproxy-configure-secure-communication.md) när du har konfigurerat omvänd proxy ska lyssna på HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-134">For HTTPS forwarding, refer to [Connect to a secure service with the reverse proxy](service-fabric-reverseproxy-configure-secure-communication.md) once you have reverse proxy setup to listen on HTTPS.</span></span>
* <span data-ttu-id="1b0ce-135">**Klustret fullständigt kvalificerade domännamnet (FQDN) | intern IP-adress:** för externa klienter kan du konfigurera omvänd proxy så att den kan nås via klusterdomänen, till exempel mycluster.eastus.cloudapp.azure.com. Som standard körs omvänd proxy på varje nod.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-135">**Cluster fully qualified domain name (FQDN) | internal IP:** For external clients, you can configure the reverse proxy so that it is reachable through the cluster domain, such as mycluster.eastus.cloudapp.azure.com. By default, the reverse proxy runs on every node.</span></span> <span data-ttu-id="1b0ce-136">För intern trafik kan omvänd proxy nås på localhost eller på alla interna noden IP-adresser, t.ex 10.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-136">For internal traffic, the reverse proxy can be reached on localhost or on any internal node IP, such as 10.0.0.1.</span></span>
* <span data-ttu-id="1b0ce-137">**Port:** porten, till exempel 19081, som har angetts för omvänd proxy.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-137">**Port:** This is the port, such as 19081, that has been specified for the reverse proxy.</span></span>
* <span data-ttu-id="1b0ce-138">**ServiceInstanceName:** detta är det fullständigt kvalificerade namnet på den distribuerade tjänst-instans som du försöker nå utan den ”fabric: /” schema.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-138">**ServiceInstanceName:** This is the fully-qualified name of the deployed service instance that you are trying to reach without the "fabric:/" scheme.</span></span> <span data-ttu-id="1b0ce-139">Till exempel för att nå den *fabric: / myapp/myservice/* tjänsten, som du vill använda *myapp/myservice*.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-139">For example, to reach the *fabric:/myapp/myservice/* service, you would use *myapp/myservice*.</span></span>

    <span data-ttu-id="1b0ce-140">Service-instansen är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-140">The service instance name is case-sensitive.</span></span> <span data-ttu-id="1b0ce-141">Med hjälp av ett annat skiftläge för tjänsten instansnamnet i URL: en medför begäranden att misslyckas med 404 (inget hittas).</span><span class="sxs-lookup"><span data-stu-id="1b0ce-141">Using a different casing for the service instance name in the URL causes the requests to fail with 404 (Not Found).</span></span>
* <span data-ttu-id="1b0ce-142">**Suffix sökväg:** detta är den faktiska URL-sökvägen som *myapi/värden/Lägg till/3*, för tjänsten som du vill ansluta till.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-142">**Suffix path:** This is the actual URL path, such as *myapi/values/add/3*, for the service that you want to connect to.</span></span>
* <span data-ttu-id="1b0ce-143">**PartitionKey:** för en partitionerad tjänst är beräknad Partitionsnyckeln för den partition som du vill nå.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-143">**PartitionKey:** For a partitioned service, this is the computed partition key of the partition that you want to reach.</span></span> <span data-ttu-id="1b0ce-144">Observera att detta *inte* partitions-ID-GUID.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-144">Note that this is *not* the partition ID GUID.</span></span> <span data-ttu-id="1b0ce-145">Den här parametern krävs inte för tjänster som använder partitionsschema singleton.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-145">This parameter is not required for services that use the singleton partition scheme.</span></span>
* <span data-ttu-id="1b0ce-146">**PartitionKind:** detta är partitionsschema för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-146">**PartitionKind:** This is the service partition scheme.</span></span> <span data-ttu-id="1b0ce-147">Detta kan vara 'Int64Range' eller 'Med namnet'.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-147">This can be 'Int64Range' or 'Named'.</span></span> <span data-ttu-id="1b0ce-148">Den här parametern krävs inte för tjänster som använder partitionsschema singleton.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-148">This parameter is not required for services that use the singleton partition scheme.</span></span>
* <span data-ttu-id="1b0ce-149">**ListenerName** slutpunkter från tjänsten är i formatet {”slutpunkter”: {”Listener1”: ”slutpunkt 1”, ”Listener2”: ”Endpoint2”...}}.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-149">**ListenerName** The endpoints from the service are of the form {"Endpoints":{"Listener1":"Endpoint1","Listener2":"Endpoint2" ...}}.</span></span> <span data-ttu-id="1b0ce-150">När tjänsten visar flera slutpunkter, identifierar den slutpunkt som klientbegäran ska vidarebefordras till.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-150">When the service exposes multiple endpoints, this identifies the endpoint that the client request should be forwarded to.</span></span> <span data-ttu-id="1b0ce-151">Detta kan utelämnas om tjänsten har endast en lyssnare.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-151">This can be omitted if the service has only one listener.</span></span>
* <span data-ttu-id="1b0ce-152">**TargetReplicaSelector** anger hur replikuppsättningens eller instans måste väljas.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-152">**TargetReplicaSelector** This specifies how the target replica or instance should be selected.</span></span>
  * <span data-ttu-id="1b0ce-153">När Måltjänsten är tillståndskänslig TargetReplicaSelector kan vara något av följande: 'PrimaryReplica', 'RandomSecondaryReplica' eller 'RandomReplica'.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-153">When the target service is stateful, the TargetReplicaSelector can be one of the following:  'PrimaryReplica', 'RandomSecondaryReplica', or 'RandomReplica'.</span></span> <span data-ttu-id="1b0ce-154">Om den här parametern inte anges är standardvärdet 'PrimaryReplica'.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-154">When this parameter is not specified, the default is 'PrimaryReplica'.</span></span>
  * <span data-ttu-id="1b0ce-155">När Måltjänsten är tillståndslös hämtar en slumpmässig instans av tjänsten partitionen att vidarebefordra begäran till omvänd proxy.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-155">When the target service is stateless, reverse proxy picks a random instance of the service partition to forward the request to.</span></span>
* <span data-ttu-id="1b0ce-156">**Timeout:** anger timeout för HTTP-begäran som skapats av omvänd proxy till tjänsten för klientbegäran.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-156">**Timeout:**  This specifies the timeout for the HTTP request created by the reverse proxy to the service on behalf of the client request.</span></span> <span data-ttu-id="1b0ce-157">Standardvärdet är 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-157">The default value is 60 seconds.</span></span> <span data-ttu-id="1b0ce-158">Det här är en valfri parameter.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-158">This is an optional parameter.</span></span>

### <a name="example-usage"></a><span data-ttu-id="1b0ce-159">Exempel på användning</span><span class="sxs-lookup"><span data-stu-id="1b0ce-159">Example usage</span></span>
<span data-ttu-id="1b0ce-160">Exempelvis ta den *fabric: / MyApp/MyService* tjänst som öppnar en HTTP-lyssnare på följande URL:</span><span class="sxs-lookup"><span data-stu-id="1b0ce-160">As an example, let's take the *fabric:/MyApp/MyService* service that opens an HTTP listener on the following URL:</span></span>

```
http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

<span data-ttu-id="1b0ce-161">Följande är resurserna för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="1b0ce-161">Following are the resources for the service:</span></span>

* `/index.html`
* `/api/users/<userId>`

<span data-ttu-id="1b0ce-162">Om tjänsten använder singleton partitioneringsschema, den *PartitionKey* och *PartitionKind* frågan string-parametrar är inte obligatoriska och tjänsten kan nås via en gateway som:</span><span class="sxs-lookup"><span data-stu-id="1b0ce-162">If the service uses the singleton partitioning scheme, the *PartitionKey* and *PartitionKind* query string parameters are not required, and the service can be reached by using the gateway as:</span></span>

* <span data-ttu-id="1b0ce-163">Externt:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`</span><span class="sxs-lookup"><span data-stu-id="1b0ce-163">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`</span></span>
* <span data-ttu-id="1b0ce-164">Internt:`http://localhost:19081/MyApp/MyService`</span><span class="sxs-lookup"><span data-stu-id="1b0ce-164">Internally: `http://localhost:19081/MyApp/MyService`</span></span>

<span data-ttu-id="1b0ce-165">Om tjänsten använder partitioneringsschema Uniform Int64 den *PartitionKey* och *PartitionKind* frågan string-parametrar måste användas för att nå en partition av tjänsten:</span><span class="sxs-lookup"><span data-stu-id="1b0ce-165">If the service uses the Uniform Int64 partitioning scheme, the *PartitionKey* and *PartitionKind* query string parameters must be used to reach a partition of the service:</span></span>

* <span data-ttu-id="1b0ce-166">Externt:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="1b0ce-166">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span></span>
* <span data-ttu-id="1b0ce-167">Internt:`http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="1b0ce-167">Internally: `http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span></span>

<span data-ttu-id="1b0ce-168">För att nå de resurser som tjänsten visar bara placera resursens sökväg efter tjänstnamnet i URL-Adressen:</span><span class="sxs-lookup"><span data-stu-id="1b0ce-168">To reach the resources that the service exposes, simply place the resource path after the service name in the URL:</span></span>

* <span data-ttu-id="1b0ce-169">Externt:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="1b0ce-169">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`</span></span>
* <span data-ttu-id="1b0ce-170">Internt:`http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="1b0ce-170">Internally: `http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`</span></span>

<span data-ttu-id="1b0ce-171">Gatewayen kommer sedan att vidarebefordra dessa begäranden till tjänstens URL:</span><span class="sxs-lookup"><span data-stu-id="1b0ce-171">The gateway will then forward these requests to the service's URL:</span></span>

* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a><span data-ttu-id="1b0ce-172">Särskild hantering för delning av port tjänster</span><span class="sxs-lookup"><span data-stu-id="1b0ce-172">Special handling for port-sharing services</span></span>
<span data-ttu-id="1b0ce-173">Azure Application Gateway försöker lösa en tjänstadress igen och försök begäran när en tjänst inte kan nås.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-173">Azure Application Gateway attempts to resolve a service address again and retry the request when a service cannot be reached.</span></span> <span data-ttu-id="1b0ce-174">Detta är en större fördel av Programgateway eftersom klienten inte behöver implementerar sin egen tjänst-lösning och lösa loop.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-174">This is a major benefit of Application Gateway because client code does not need to implement its own service resolution and resolve loop.</span></span>

<span data-ttu-id="1b0ce-175">I allmänhet när en tjänst kan inte nås, service-instans eller replik har flyttats till en annan nod som en del av sin normala livscykel.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-175">Generally, when a service cannot be reached, the service instance or replica has moved to a different node as part of its normal lifecycle.</span></span> <span data-ttu-id="1b0ce-176">När detta inträffar kan Programgateway får ett fel i anslutningen som anger att en slutpunkt är inte längre öppna den ursprungligen matcha adressen.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-176">When this happens, Application Gateway might receive a network connection error indicating that an endpoint is no longer open on the originally resolved address.</span></span>

<span data-ttu-id="1b0ce-177">Dock replikeringar eller instanser av tjänsten kan dela en värdprocess och kan också dela en port när finns en http.sys-baserade webbservern, inklusive:</span><span class="sxs-lookup"><span data-stu-id="1b0ce-177">However, replicas or service instances can share a host process and might also share a port when hosted by an http.sys-based web server, including:</span></span>

* [<span data-ttu-id="1b0ce-178">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="1b0ce-178">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
* [<span data-ttu-id="1b0ce-179">ASP.NET Core WebListener</span><span class="sxs-lookup"><span data-stu-id="1b0ce-179">ASP.NET Core WebListener</span></span>](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
* [<span data-ttu-id="1b0ce-180">Katana</span><span class="sxs-lookup"><span data-stu-id="1b0ce-180">Katana</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

<span data-ttu-id="1b0ce-181">I det här fallet är det troligt att webbservern är tillgängliga i värdprocessen och svarar på begäran, men löst tjänstinstansen eller repliken inte längre tillgänglig på värden.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-181">In this situation, it is likely that the web server is available in the host process and responding to requests, but the resolved service instance or replica is no longer available on the host.</span></span> <span data-ttu-id="1b0ce-182">I det här fallet får gatewayen ett HTTP 404-svar från webbservern.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-182">In this case, the gateway will receive an HTTP 404 response from the web server.</span></span> <span data-ttu-id="1b0ce-183">Därför har en HTTP 404 två distinkta innebörd:</span><span class="sxs-lookup"><span data-stu-id="1b0ce-183">Thus, an HTTP 404 has two distinct meanings:</span></span>

- <span data-ttu-id="1b0ce-184">Fall #1: serviceadressen är korrekt, men den resurs som användaren begärde finns inte.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-184">Case #1: The service address is correct, but the resource that the user requested does not exist.</span></span>
- <span data-ttu-id="1b0ce-185">Fall #2: serviceadressen är felaktig och resursen som användaren har begärt kan finnas på en annan nod.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-185">Case #2: The service address is incorrect, and the resource that the user requested might exist on a different node.</span></span>

<span data-ttu-id="1b0ce-186">Det första fallet är en normal HTTP 404 som anses vara ett användarfel.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-186">The first case is a normal HTTP 404, which is considered a user error.</span></span> <span data-ttu-id="1b0ce-187">I det andra fallet har dock användaren begärde en resurs som finns.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-187">However, in the second case, the user has requested a resource that does exist.</span></span> <span data-ttu-id="1b0ce-188">Programgateway gick inte att hitta den eftersom själva tjänsten har flyttats.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-188">Application Gateway was unable to locate it because the service itself has moved.</span></span> <span data-ttu-id="1b0ce-189">Programgateway måste matcha adressen igen och försök sedan begäran.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-189">Application Gateway needs to resolve the address again and retry the request.</span></span>

<span data-ttu-id="1b0ce-190">Programgateway måste därför kan skilja mellan dessa två fall.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-190">Application Gateway thus needs a way to distinguish between these two cases.</span></span> <span data-ttu-id="1b0ce-191">För att göra denna skillnad, krävs en ledtråd från servern.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-191">To make that distinction, a hint from the server is required.</span></span>

* <span data-ttu-id="1b0ce-192">Som standard Programgateway förutsätter fall #2 och försöker lösa och skicka begäran igen.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-192">By default, Application Gateway assumes case #2 and attempts to resolve and issue the request again.</span></span>
* <span data-ttu-id="1b0ce-193">Om du vill ange fall #1 till Programgateway ska tjänsten returnera följande HTTP-Svarsrubrik:</span><span class="sxs-lookup"><span data-stu-id="1b0ce-193">To indicate case #1 to Application Gateway, the service should return the following HTTP response header:</span></span>

  `X-ServiceFabric : ResourceNotFound`

<span data-ttu-id="1b0ce-194">Den här HTTP-Svarsrubrik visar en normal HTTP 404-situation där den begärda resursen finns inte och Programgateway försöker inte matcha tjänstadressen igen.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-194">This HTTP response header indicates a normal HTTP 404 situation in which the requested resource does not exist, and Application Gateway will not attempt to resolve the service address again.</span></span>

## <a name="setup-and-configuration"></a><span data-ttu-id="1b0ce-195">Installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="1b0ce-195">Setup and configuration</span></span>

### <a name="enable-reverse-proxy-via-azure-portal"></a><span data-ttu-id="1b0ce-196">Aktivera omvänd proxy via Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1b0ce-196">Enable reverse proxy via Azure portal</span></span>

<span data-ttu-id="1b0ce-197">Azure-portalen innehåller ett alternativ för att aktivera omvänd proxy medan du skapar ett nytt Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-197">Azure portal provides an option to enable Reverse proxy while creating a new Service Fabric cluster.</span></span>
<span data-ttu-id="1b0ce-198">Under **skapar Service Fabric-kluster**, steg 2: klusterkonfiguration, konfiguration av noden typ, markera kryssrutan ”Aktivera omvänd proxy”.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-198">Under **Create Service Fabric cluster**, Step 2: Cluster Configuration, Node type configuration, select the checkbox to "Enable reverse proxy".</span></span>
<span data-ttu-id="1b0ce-199">För att konfigurera säker omvänd proxy, SSL-certifikat kan anges i steg3: säkerhet, konfigurera säkerhetsinställningar för klustret, markera kryssrutan för att ”innehåller ett SSL-certifikat för omvänd proxy” och ange information om certifikat.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-199">For configuring secure reverse proxy, SSL certificate can be specified in Step 3: Security, Configure cluster security settings, select the checkbox to "Include a SSL certificate for reverse proxy" and enter the certificate details.</span></span>

### <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a><span data-ttu-id="1b0ce-200">Aktivera omvänd proxy via Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="1b0ce-200">Enable reverse proxy via Azure Resource Manager templates</span></span>

<span data-ttu-id="1b0ce-201">Du kan använda den [Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md) att aktivera omvänd proxy i Service Fabric för klustret.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-201">You can use the [Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md) to enable the reverse proxy in Service Fabric for the cluster.</span></span>

<span data-ttu-id="1b0ce-202">Referera till [konfigurera HTTPS omvänd Proxy i ett kluster för säker](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) för Azure Resource Manager mallen exempel för att konfigurera säker omvänd proxy med förnya ett certifikat och hantering av certifikatet.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-202">Refer to [Configure HTTPS Reverse Proxy in a secure cluster](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) for Azure Resource Manager template samples to configure secure reverse proxy with a certificate and handling certificate rollover.</span></span>

<span data-ttu-id="1b0ce-203">Först måste hämta du mallen för det kluster som du vill distribuera.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-203">First, you get the template for the cluster that you want to deploy.</span></span> <span data-ttu-id="1b0ce-204">Du kan använda exempelmallarna, eller så kan du skapa en anpassad mall för hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-204">You can either use the sample templates or create a custom Resource Manager template.</span></span> <span data-ttu-id="1b0ce-205">Sedan kan du aktivera omvänd proxy med hjälp av följande steg:</span><span class="sxs-lookup"><span data-stu-id="1b0ce-205">Then, you can enable the reverse proxy by using the following steps:</span></span>

1. <span data-ttu-id="1b0ce-206">Definiera en port för omvänd proxy i den [parametrar avsnittet](../azure-resource-manager/resource-group-authoring-templates.md) för mallen.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-206">Define a port for the reverse proxy in the [Parameters section](../azure-resource-manager/resource-group-authoring-templates.md) of the template.</span></span>

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. <span data-ttu-id="1b0ce-207">Ange porten för varje nodetype objekten i den **klustret** [typen avsnittet](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1b0ce-207">Specify the port for each of the nodetype objects in the **Cluster** [Resource type section](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

    <span data-ttu-id="1b0ce-208">Porten som identifieras av parameternamn, reverseProxyEndpointPort.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-208">The port is identified by the parameter name, reverseProxyEndpointPort.</span></span>

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
3. <span data-ttu-id="1b0ce-209">Ställ in Azure belastningsutjämnare regler för den port som du angav i steg 1 för att åtgärda omvänd proxy från utanför Azure klustret.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-209">To address the reverse proxy from outside the Azure cluster, set up the Azure Load Balancer rules for the port that you specified in step 1.</span></span>

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
4. <span data-ttu-id="1b0ce-210">Konfigurera SSL-certifikat på porten för omvänd proxy genom att lägga till certifikatet till den ***reverseProxyCertificate*** egenskap i den **klustret** [typen avsnittet](../resource-group-authoring-templates.md) .</span><span class="sxs-lookup"><span data-stu-id="1b0ce-210">To configure SSL certificates on the port for the reverse proxy, add the certificate to the ***reverseProxyCertificate*** property in the **Cluster** [Resource type section](../resource-group-authoring-templates.md).</span></span>

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

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-the-cluster-certificate"></a><span data-ttu-id="1b0ce-211">Stöd för en omvänd proxy-certifikat som skiljer sig från klustret certifikatet</span><span class="sxs-lookup"><span data-stu-id="1b0ce-211">Supporting a reverse proxy certificate that's different from the cluster certificate</span></span>
 <span data-ttu-id="1b0ce-212">Om certifikatet omvänd proxy skiljer sig från det certifikat som skyddar klustret ska tidigare angivna certifikatet installeras på den virtuella datorn och därefter lagt till i åtkomstkontrollistan (ACL) så att Service Fabric kan komma åt den.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-212">If the reverse proxy certificate is different from the certificate that secures the cluster, then the previously specified certificate should be installed on the virtual machine and added to the access control list (ACL) so that Service Fabric can access it.</span></span> <span data-ttu-id="1b0ce-213">Detta kan göras den **virtualMachineScaleSets** [typen avsnittet](../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1b0ce-213">This can be done in the **virtualMachineScaleSets** [Resource type section](../resource-group-authoring-templates.md).</span></span> <span data-ttu-id="1b0ce-214">Lägga till certifikatet i osProfile för installation.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-214">For installation, add that certificate to the osProfile.</span></span> <span data-ttu-id="1b0ce-215">Avsnittet tillägg i mallen kan uppdatera certifikat i Åtkomstkontrollistan.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-215">The extension section of the template can update the certificate in the ACL.</span></span>

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
> <span data-ttu-id="1b0ce-216">När du använder certifikat som skiljer sig från klustret certifikat för att aktivera omvänd proxy på ett befintligt kluster, installera certifikat för omvänd proxy och uppdatera ACL på klustret innan du aktiverar omvänd proxy.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-216">When you use certificates that are different from the cluster certificate to enable the reverse proxy on an existing cluster, install the reverse proxy certificate and update the ACL on the cluster before you enable the reverse proxy.</span></span> <span data-ttu-id="1b0ce-217">Slutför den [Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md) distribution med hjälp av inställningarna som nämnts tidigare innan du startar en distribution för att aktivera omvänd proxy i steg 1 – 4.</span><span class="sxs-lookup"><span data-stu-id="1b0ce-217">Complete the [Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md) deployment by using the settings mentioned previously before you start a deployment to enable the reverse proxy in steps 1-4.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b0ce-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1b0ce-218">Next steps</span></span>
* <span data-ttu-id="1b0ce-219">Se ett exempel på HTTP-kommunikation mellan tjänster i en [exempelprojektet på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="1b0ce-219">See an example of HTTP communication between services in a [sample project on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span></span>
* [<span data-ttu-id="1b0ce-220">Vidarebefordran till säker HTTP-tjänsten med omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="1b0ce-220">Forwarding to secure HTTP service with the reverse proxy</span></span>](service-fabric-reverseproxy-configure-secure-communication.md)
* [<span data-ttu-id="1b0ce-221">RPC-anrop med Reliable Services fjärrkommunikation</span><span class="sxs-lookup"><span data-stu-id="1b0ce-221">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="1b0ce-222">Webb-API som använder OWIN i Reliable Services</span><span class="sxs-lookup"><span data-stu-id="1b0ce-222">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="1b0ce-223">WCF-kommunikation med hjälp av Reliable Services</span><span class="sxs-lookup"><span data-stu-id="1b0ce-223">WCF communication by using Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
* <span data-ttu-id="1b0ce-224">Ytterligare omvänd proxy konfigurationsalternativ finns ApplicationGateway/http-avsnittet i [anpassa Service Fabric-klusterinställningar](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="1b0ce-224">For additional reverse proxy configuration options, refer ApplicationGateway/Http section in [Customize Service Fabric cluster settings](service-fabric-cluster-fabric-settings.md).</span></span>

[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
