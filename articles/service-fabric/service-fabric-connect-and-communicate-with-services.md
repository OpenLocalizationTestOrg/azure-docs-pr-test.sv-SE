---
title: "Ansluta och kommunicera med tjänster i Azure Service Fabric | Microsoft Docs"
description: "Lär dig hur du löser ansluta och kommunicera med tjänster i Service Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: msfussell
ms.assetid: 7d1052ec-2c9f-443d-8b99-b75c97266e6c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/9/2017
ms.author: vturecek
ms.openlocfilehash: 3e61ad19df34c6a57da43e26bd2ab9d7ecdbf98e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-and-communicate-with-services-in-service-fabric"></a><span data-ttu-id="6c655-103">Ansluta och kommunicera med tjänster i Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6c655-103">Connect and communicate with services in Service Fabric</span></span>
<span data-ttu-id="6c655-104">I Service Fabric körs en tjänst någonstans i ett Service Fabric-kluster, vanligtvis fördelade på flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6c655-104">In Service Fabric, a service runs somewhere in a Service Fabric cluster, typically distributed across multiple VMs.</span></span> <span data-ttu-id="6c655-105">Den kan flyttas från en plats till en annan, antingen av tjänstens ägare eller automatiskt av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6c655-105">It can be moved from one place to another, either by the service owner, or automatically by Service Fabric.</span></span> <span data-ttu-id="6c655-106">Tjänster är inte statiskt knutna till en viss dator eller en adress.</span><span class="sxs-lookup"><span data-stu-id="6c655-106">Services are not statically tied to a particular machine or address.</span></span>

<span data-ttu-id="6c655-107">Ett Service Fabric-program består normalt av många olika tjänster, där varje tjänst utför en särskild aktivitet.</span><span class="sxs-lookup"><span data-stu-id="6c655-107">A Service Fabric application is generally composed of many different services, where each service performs a specialized task.</span></span> <span data-ttu-id="6c655-108">Dessa tjänster kan kommunicera med varandra för att bilda en fullständig funktion, till exempel återgivning olika delar av ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="6c655-108">These services may communicate with each other to form a complete function, such as rendering different parts of a web application.</span></span> <span data-ttu-id="6c655-109">Det finns också klientprogram som kan ansluta till och kommunicera med tjänster.</span><span class="sxs-lookup"><span data-stu-id="6c655-109">There are also client applications that connect to and communicate with services.</span></span> <span data-ttu-id="6c655-110">Det här dokumentet beskrivs hur du ställer in kommunikation med och mellan dina tjänster i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6c655-110">This document discusses how to set up communication with and between your services in Service Fabric.</span></span>

<span data-ttu-id="6c655-111">Den här Microsoft Virtual Academy videon beskrivs också kommunikation:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965"></span><span class="sxs-lookup"><span data-stu-id="6c655-111">This Microsoft Virtual Academy video also discusses service communication: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965"></span></span>  
<img src="./media/service-fabric-connect-and-communicate-with-services/CommunicationVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="bring-your-own-protocol"></a><span data-ttu-id="6c655-112">Ta med egna protokoll</span><span class="sxs-lookup"><span data-stu-id="6c655-112">Bring your own protocol</span></span>
<span data-ttu-id="6c655-113">Service Fabric hjälper dig att hantera livscykeln för dina tjänster, men det gör inte beslut om dina tjänster gör.</span><span class="sxs-lookup"><span data-stu-id="6c655-113">Service Fabric helps manage the lifecycle of your services but it does not make decisions about what your services do.</span></span> <span data-ttu-id="6c655-114">Detta omfattar kommunikation.</span><span class="sxs-lookup"><span data-stu-id="6c655-114">This includes communication.</span></span> <span data-ttu-id="6c655-115">När tjänsten har öppnats av Service Fabric, som är din tjänst möjlighet att ställa in en slutpunkt för inkommande begäranden som använder oavsett protokoll eller kommunikation stack som du vill.</span><span class="sxs-lookup"><span data-stu-id="6c655-115">When your service is opened by Service Fabric, that's your service's opportunity to set up an endpoint for incoming requests, using whatever protocol or communication stack you want.</span></span> <span data-ttu-id="6c655-116">Tjänsten kommer att lyssna på en normal **IP:port** med adresseringsschema, till exempel en URI-adress.</span><span class="sxs-lookup"><span data-stu-id="6c655-116">Your service will listen on a normal **IP:port** address using any addressing scheme, such as a URI.</span></span> <span data-ttu-id="6c655-117">Flera instanser av tjänsten eller repliker kan dela en värdprocess, då de antingen måste använda olika portar eller använda en delning av port mekanism, till exempel http.sys kernel-drivrutinen i Windows.</span><span class="sxs-lookup"><span data-stu-id="6c655-117">Multiple service instances or replicas may share a host process, in which case they will either need to use different ports or use a port-sharing mechanism, such as the http.sys kernel driver in Windows.</span></span> <span data-ttu-id="6c655-118">I båda fallen måste varje instans av tjänsten eller replik i en värdprocess måste vara unikt adresserbara.</span><span class="sxs-lookup"><span data-stu-id="6c655-118">In either case, each service instance or replica in a host process must be uniquely addressable.</span></span>

![slutpunkter][1]

## <a name="service-discovery-and-resolution"></a><span data-ttu-id="6c655-120">Identifiering och lösning</span><span class="sxs-lookup"><span data-stu-id="6c655-120">Service discovery and resolution</span></span>
<span data-ttu-id="6c655-121">I ett distribuerat system, kan tjänster flytta från en dator till en annan över tid.</span><span class="sxs-lookup"><span data-stu-id="6c655-121">In a distributed system, services may move from one machine to another over time.</span></span> <span data-ttu-id="6c655-122">Detta kan inträffa av olika skäl, inklusive resurser, uppgraderingar, redundans och skalbara.</span><span class="sxs-lookup"><span data-stu-id="6c655-122">This can happen for various reasons, including resource balancing, upgrades, failovers, or scale-out.</span></span> <span data-ttu-id="6c655-123">Det innebär att tjänsten slutpunkts-adresser ändras när tjänsten flyttas till noder med olika IP-adresser och öppnas på olika portar om tjänsten använder en dynamiskt vald port.</span><span class="sxs-lookup"><span data-stu-id="6c655-123">This means service endpoint addresses change as the service moves to nodes with different IP addresses, and may open on different ports if the service uses a dynamically selected port.</span></span>

![Distribution av tjänster][7]

<span data-ttu-id="6c655-125">Service Fabric är en tjänst för identifiering och lösning som kallas Naming Service.</span><span class="sxs-lookup"><span data-stu-id="6c655-125">Service Fabric provides a discovery and resolution service called the Naming Service.</span></span> <span data-ttu-id="6c655-126">Tjänsten Naming upprätthåller en tabell som mappar namngivna instanser av tjänsten till slutpunkts-adresser som de lyssna på.</span><span class="sxs-lookup"><span data-stu-id="6c655-126">The Naming Service maintains a table that maps named service instances to the endpoint addresses they listen on.</span></span> <span data-ttu-id="6c655-127">Alla instanser för tjänsten i Service Fabric har unika namn som visas i form av URI: er, till exempel `"fabric:/MyApplication/MyService"`.</span><span class="sxs-lookup"><span data-stu-id="6c655-127">All named service instances in Service Fabric have unique names represented as URIs, for example, `"fabric:/MyApplication/MyService"`.</span></span> <span data-ttu-id="6c655-128">Namnet på tjänsten ändras inte under livslängden för tjänsten, är det bara slutpunkts-adresser som kan ändras när tjänsterna flytta.</span><span class="sxs-lookup"><span data-stu-id="6c655-128">The name of the service does not change over the lifetime of the service, it's only the endpoint addresses that can change when services move.</span></span> <span data-ttu-id="6c655-129">Detta är detsamma som webbplatser som har konstant URL: er men där IP-adress kan ändras.</span><span class="sxs-lookup"><span data-stu-id="6c655-129">This is analogous to websites that have constant URLs but where the IP address may change.</span></span> <span data-ttu-id="6c655-130">Och liknar DNS på webben, som matchar IP-adresser för webbplats-URL: er, Service Fabric har ett register som mappar tjänstnamn till deras slutpunktsadress.</span><span class="sxs-lookup"><span data-stu-id="6c655-130">And similar to DNS on the web, which resolves website URLs to IP addresses, Service Fabric has a registrar that maps service names to their endpoint address.</span></span>

![slutpunkter][2]

<span data-ttu-id="6c655-132">Lösa och ansluta till tjänster omfattar följande steg i en slinga:</span><span class="sxs-lookup"><span data-stu-id="6c655-132">Resolving and connecting to services involves the following steps run in a loop:</span></span>

* <span data-ttu-id="6c655-133">**Lös**: hämta den slutpunkt som har publicerat en tjänst från Naming Service.</span><span class="sxs-lookup"><span data-stu-id="6c655-133">**Resolve**: Get the endpoint that a service has published from the Naming Service.</span></span>
* <span data-ttu-id="6c655-134">**Ansluta**: ansluta till tjänsten via oavsett protokoll använder på denna slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="6c655-134">**Connect**: Connect to the service over whatever protocol it uses on that endpoint.</span></span>
* <span data-ttu-id="6c655-135">**Försök**: ett anslutningsförsök misslyckas av olika orsaker, för om tjänsten har flyttats sedan den senast slutpunktsadressen löstes.</span><span class="sxs-lookup"><span data-stu-id="6c655-135">**Retry**: A connection attempt may fail for any number of reasons, for example if the service has moved since the last time the endpoint address was resolved.</span></span> <span data-ttu-id="6c655-136">I så fall den föregående lösa och ansluta steg måste göras och den här cykeln upprepas tills anslutningen lyckas.</span><span class="sxs-lookup"><span data-stu-id="6c655-136">In that case, the preceding resolve and connect steps need to be retried, and this cycle is repeated until the connection succeeds.</span></span>

## <a name="connecting-to-other-services"></a><span data-ttu-id="6c655-137">Ansluter till andra tjänster</span><span class="sxs-lookup"><span data-stu-id="6c655-137">Connecting to other services</span></span>
<span data-ttu-id="6c655-138">Tjänster vanligtvis ansluta till varandra i ett kluster kan komma åt direkt slutpunkter av andra tjänster eftersom noder i ett kluster finns i samma lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="6c655-138">Services connecting to each other inside a cluster generally can directly access the endpoints of other services because the nodes in a cluster are on the same local network.</span></span> <span data-ttu-id="6c655-139">Om du vill göra är enklare att ansluta olika tjänster, Service Fabric ger ytterligare tjänster som använder Naming Service.</span><span class="sxs-lookup"><span data-stu-id="6c655-139">To make is easier to connect between services, Service Fabric provides additional services that use the Naming Service.</span></span> <span data-ttu-id="6c655-140">En DNS-tjänst och en omvänd proxy-tjänst.</span><span class="sxs-lookup"><span data-stu-id="6c655-140">A DNS service and a reverse proxy service.</span></span>


### <a name="dns-service"></a><span data-ttu-id="6c655-141">DNS-tjänst</span><span class="sxs-lookup"><span data-stu-id="6c655-141">DNS service</span></span>
<span data-ttu-id="6c655-142">Eftersom många tjänster, särskilt av tjänster kan ha ett befintligt URL-namn, att kunna lösa dessa med hjälp av standard-DNS-protokollet (i stället för protokollet Naming Service) är mycket praktiskt, särskilt i programmet ”lyfta och flytta” scenarier.</span><span class="sxs-lookup"><span data-stu-id="6c655-142">Since many services, especially containerized services, can have an existing URL name, being able to resolve these using the standard DNS protocol (rather than the Naming Service protocol) is very convenient, especially in application "lift and shift" scenarios.</span></span> <span data-ttu-id="6c655-143">Detta är exakt det DNS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6c655-143">This is exactly what the DNS service does.</span></span> <span data-ttu-id="6c655-144">På så sätt kan du mappa DNS-namn till ett namn och därför matcha IP-adresser för slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="6c655-144">It enables you to map DNS names to a service name and hence resolve endpoint IP addresses.</span></span> 

<span data-ttu-id="6c655-145">I följande diagram visas matchar DNS-namn till tjänstnamn som sedan kan matchas med namngivningstjänst att returnera slutpunkts-adresser för att ansluta till DNS-tjänsten som körs i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="6c655-145">As shown in the following diagram, the DNS service, running in the Service Fabric cluster, maps DNS names to service names which are then resolved by the Naming Service to return the endpoint addresses to connect to.</span></span> <span data-ttu-id="6c655-146">DNS-namn för tjänsten tillhandahålls vid tidpunkten för skapandet.</span><span class="sxs-lookup"><span data-stu-id="6c655-146">The DNS name for the service is provided at the time of creation.</span></span> 

![slutpunkter][9]

<span data-ttu-id="6c655-148">För mer information om hur du använder DNS-tjänsten finns [DNS-tjänsten i Azure Service Fabric](service-fabric-dnsservice.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="6c655-148">For more details on how to use the DNS service see [DNS service in Azure Service Fabric](service-fabric-dnsservice.md) article.</span></span>

### <a name="reverse-proxy-service"></a><span data-ttu-id="6c655-149">Tjänsten omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="6c655-149">Reverse proxy service</span></span>
<span data-ttu-id="6c655-150">Omvänd proxy adresser tjänster i klustret som exponerar http-slutpunkter, inklusive HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6c655-150">The reverse proxy addresses services in the cluster that exposes HTTP endpoints including HTTPS.</span></span> <span data-ttu-id="6c655-151">Omvänd proxy avsevärt förenklar anropa andra tjänster och deras metoder genom att ha ett specifikt URI-format och hanterar lösas, ansluta, försök steg som krävs för en tjänst att kommunicera med varandra med namngivning av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6c655-151">The reverse proxy greatly simplifies calling other services and their methods by having a specific URI format and handles the resolve, connect, retry steps required for one service to communicate with another using the Naming Serivce.</span></span> <span data-ttu-id="6c655-152">Med andra ord döljer namngivningstjänst från dig vid anrop av andra tjänster genom att göra detta så enkelt som anropar en URL.</span><span class="sxs-lookup"><span data-stu-id="6c655-152">In other words, it hides the Naming Service from you when calling other services by making this as simple as calling a URL.</span></span>

![slutpunkter][10]

<span data-ttu-id="6c655-154">Mer information om hur du använder omvänd proxy-tjänst finns [omvänd proxy i Azure Service Fabric](service-fabric-reverseproxy.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="6c655-154">For more details on how to use the reverse proxy service see [Reverse proxy in Azure Service Fabric](service-fabric-reverseproxy.md) article.</span></span>

## <a name="connections-from-external-clients"></a><span data-ttu-id="6c655-155">Anslutningar från externa klienter</span><span class="sxs-lookup"><span data-stu-id="6c655-155">Connections from external clients</span></span>
<span data-ttu-id="6c655-156">Tjänster vanligtvis ansluta till varandra i ett kluster kan komma åt direkt slutpunkter av andra tjänster eftersom noder i ett kluster finns i samma lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="6c655-156">Services connecting to each other inside a cluster generally can directly access the endpoints of other services because the nodes in a cluster are on the same local network.</span></span> <span data-ttu-id="6c655-157">I vissa miljöer med kan ett kluster dock finnas bakom en belastningsutjämnare som skickar externa ingång trafik via en begränsad uppsättning portar.</span><span class="sxs-lookup"><span data-stu-id="6c655-157">In some environments, however, a cluster may be behind a load balancer that routes external ingress traffic through a limited set of ports.</span></span> <span data-ttu-id="6c655-158">I dessa fall tjänster kan kommunicera med varandra och matcha adresser med att använda fortfarande, men måste du vidta ytterligare åtgärder för att tillåta externa klienter att ansluta till tjänster.</span><span class="sxs-lookup"><span data-stu-id="6c655-158">In these cases, services can still communicate with each other and resolve addresses using the Naming Service, but extra steps must be taken to allow external clients to connect to services.</span></span>

## <a name="service-fabric-in-azure"></a><span data-ttu-id="6c655-159">Service Fabric i Azure</span><span class="sxs-lookup"><span data-stu-id="6c655-159">Service Fabric in Azure</span></span>
<span data-ttu-id="6c655-160">Ett Service Fabric-kluster i Azure är placerad bakom en belastningsutjämnare i Azure.</span><span class="sxs-lookup"><span data-stu-id="6c655-160">A Service Fabric cluster in Azure is placed behind an Azure Load Balancer.</span></span> <span data-ttu-id="6c655-161">Alla externa trafik till klustret måste gå via belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="6c655-161">All external traffic to the cluster must pass through the load balancer.</span></span> <span data-ttu-id="6c655-162">Belastningsutjämnaren kommer automatiskt att vidarebefordra trafik inkommande på en viss port till ett slumpmässigt *nod* som har samma port öppna.</span><span class="sxs-lookup"><span data-stu-id="6c655-162">The load balancer will automatically forward traffic inbound on a given port to a random *node* that has the same port open.</span></span> <span data-ttu-id="6c655-163">Azure belastningsutjämnare bara medveten om portar som är öppna på de *noder*, öppnar du portar genom att individuella inte känner *services*.</span><span class="sxs-lookup"><span data-stu-id="6c655-163">The Azure Load Balancer only knows about ports open on the *nodes*, it does not know about ports open by individual *services*.</span></span>

![Azure belastningsutjämnare och Service Fabric-topologi][3]

<span data-ttu-id="6c655-165">Till exempel för att kunna acceptera extern trafik på port **80**, konfigureras följande saker:</span><span class="sxs-lookup"><span data-stu-id="6c655-165">For example, in order to accept external traffic on port **80**, the following things must be configured:</span></span>

1. <span data-ttu-id="6c655-166">Skriv en tjänst som lyssnar på port 80.</span><span class="sxs-lookup"><span data-stu-id="6c655-166">Write a service that listens on port 80.</span></span> <span data-ttu-id="6c655-167">Konfigurera port 80 i tjänstens ServiceManifest.xml och öppna en lyssnare i tjänsten, till exempel en egen värdbaserade webbserver.</span><span class="sxs-lookup"><span data-stu-id="6c655-167">Configure port 80 in the service's ServiceManifest.xml and open a listener in the service, for example, a self-hosted web server.</span></span>

    ```xml
    <Resources>
        <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>
    ```
    ```csharp
        class HttpCommunicationListener : ICommunicationListener
        {
            ...

            public Task<string> OpenAsync(CancellationToken cancellationToken)
            {
                EndpointResourceDescription endpoint =
                    serviceContext.CodePackageActivationContext.GetEndpoint("WebEndpoint");

                string uriPrefix = $"{endpoint.Protocol}://+:{endpoint.Port}/myapp/";

                this.httpListener = new HttpListener();
                this.httpListener.Prefixes.Add(uriPrefix);
                this.httpListener.Start();

                string publishUri = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
                return Task.FromResult(publishUri);
            }

            ...
        }

        class WebService : StatelessService
        {
            ...

            protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
            {
                return new[] { new ServiceInstanceListener(context => new HttpCommunicationListener(context))};
            }

            ...
        }
    ```
    ```java
        class HttpCommunicationlistener implements CommunicationListener {
            ...

            @Override
            public CompletableFuture<String> openAsync(CancellationToken arg0) {
                EndpointResourceDescription endpoint =
                    this.serviceContext.getCodePackageActivationContext().getEndpoint("WebEndpoint");
                try {
                    HttpServer server = com.sun.net.httpserver.HttpServer.create(new InetSocketAddress(endpoint.getPort()), 0);
                    server.start();

                    String publishUri = String.format("http://%s:%d/",
                        this.serviceContext.getNodeContext().getIpAddressOrFQDN(), endpoint.getPort());
                    return CompletableFuture.completedFuture(publishUri);
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }

            ...
        }

        class WebService extends StatelessService {
            ...

            @Override
            protected List<ServiceInstanceListener> createServiceInstanceListeners() {
                <ServiceInstanceListener> listeners = new ArrayList<ServiceInstanceListener>();
                listeners.add(new ServiceInstanceListener((context) -> new HttpCommunicationlistener(context)));
                return listeners;       
            }

            ...
        }
    ```
2. <span data-ttu-id="6c655-168">Skapa ett Service Fabric-kluster i Azure och ange port **80** som en anpassad endpoint-port för nodtypen som är värd för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6c655-168">Create a Service Fabric Cluster in Azure and specify port **80** as a custom endpoint port for the node type that will host the service.</span></span> <span data-ttu-id="6c655-169">Om du har mer än en nodtyp kan du skapa en *placering begränsningen* på tjänsten så att den kan bara köras på nodtypen som har anpassade endpoint porten öppnas.</span><span class="sxs-lookup"><span data-stu-id="6c655-169">If you have more than one node type, you can set up a *placement constraint* on the service to ensure it only runs on the node type that has the custom endpoint port opened.</span></span>

    ![Öppna en port på en nodtyp][4]
3. <span data-ttu-id="6c655-171">När klustret har skapats kan du konfigurera Azure belastningsutjämnare i klusterresursgrupp att vidarebefordra trafik på port 80.</span><span class="sxs-lookup"><span data-stu-id="6c655-171">Once the cluster has been created, configure the Azure Load Balancer in the cluster's Resource Group to forward traffic on port 80.</span></span> <span data-ttu-id="6c655-172">När du skapar ett kluster via Azure portal, ställs detta in automatiskt för varje anpassad endpoint-port som har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="6c655-172">When creating a cluster through the Azure portal, this is set up automatically for each custom endpoint port that was configured.</span></span>

    ![Vidarebefordra trafik i Azure belastningsutjämnare][5]
4. <span data-ttu-id="6c655-174">Azure belastningsutjämnare använder en avsökning för att avgöra om att skicka trafik till en viss nod eller inte.</span><span class="sxs-lookup"><span data-stu-id="6c655-174">The Azure Load Balancer uses a probe to determine whether or not to send traffic to a particular node.</span></span> <span data-ttu-id="6c655-175">Avsökningen söker regelbundet en slutpunkt på varje nod för att avgöra om noden svarar.</span><span class="sxs-lookup"><span data-stu-id="6c655-175">The probe periodically checks an endpoint on each node to determine whether or not the node is responding.</span></span> <span data-ttu-id="6c655-176">Om det inte går att ta emot ett svar efter ett visst antal gånger avsökningen, stoppar belastningsutjämnaren skickar trafik till noden.</span><span class="sxs-lookup"><span data-stu-id="6c655-176">If the probe fails to receive a response after a configured number of times, the load balancer stops sending traffic to that node.</span></span> <span data-ttu-id="6c655-177">När du skapar ett kluster via Azure portal, konfigureras automatiskt en avsökning för varje anpassad endpoint-port som har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="6c655-177">When creating a cluster through the Azure portal, a probe is automatically set up for each custom endpoint port that was configured.</span></span>

    ![Vidarebefordra trafik i Azure belastningsutjämnare][8]

<span data-ttu-id="6c655-179">Det är viktigt att komma ihåg att Azure belastningsutjämnare och avsökningen bara känna av *noder*, inte den *tjänster* körs på noderna.</span><span class="sxs-lookup"><span data-stu-id="6c655-179">It's important to remember that the Azure Load Balancer and the probe only know about the *nodes*, not the *services* running on the nodes.</span></span> <span data-ttu-id="6c655-180">Azure belastningsutjämnare kommer alltid att skicka trafik till noder som svarar avsökningen, så måste vara försiktig så att tjänster är tillgängliga på de noder som är kunna svara avsökningen.</span><span class="sxs-lookup"><span data-stu-id="6c655-180">The Azure Load Balancer will always send traffic to nodes that respond to the probe, so care must be taken to ensure services are available on the nodes that are able to respond to the probe.</span></span>

## <a name="reliable-services-built-in-communication-api-options"></a><span data-ttu-id="6c655-181">Reliable Services: Inbyggda API kommunikationsalternativ</span><span class="sxs-lookup"><span data-stu-id="6c655-181">Reliable Services: Built-in communication API options</span></span>
<span data-ttu-id="6c655-182">Reliable Services framework levereras med flera fördefinierade kommunikationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="6c655-182">The Reliable Services framework ships with several pre-built communication options.</span></span> <span data-ttu-id="6c655-183">Beslutet om vilka en fungerar bäst för dig beror på valet av programmeringsmiljö, kommunikation framework och programmeringsspråk som dina tjänster har skrivits i.</span><span class="sxs-lookup"><span data-stu-id="6c655-183">The decision about which one will work best for you depends on the choice of the programming model, the communication framework, and the programming language that your services are written in.</span></span>

* <span data-ttu-id="6c655-184">**Inget specifikt protokoll:** om du inte har ett visst val av kommunikation framework, men du vill få något dig snabbt, så det bästa alternativet för du [remoting service](service-fabric-reliable-services-communication-remoting.md), vilket gör att strikt typkontroll RPC-anrop för Reliable Services och Reliable Actors.</span><span class="sxs-lookup"><span data-stu-id="6c655-184">**No specific protocol:**  If you don't have a particular choice of communication framework, but you want to get something up and running quickly, then the ideal option for you is [service remoting](service-fabric-reliable-services-communication-remoting.md), which allows strongly-typed remote procedure calls for Reliable Services and Reliable Actors.</span></span> <span data-ttu-id="6c655-185">Detta är det enklaste och snabbaste sättet att komma igång med service-kommunikation.</span><span class="sxs-lookup"><span data-stu-id="6c655-185">This is the easiest and fastest way to get started with service communication.</span></span> <span data-ttu-id="6c655-186">Tjänsten fjärrkommunikation hanterar lösning av postadresser, anslutning, försök igen och felhantering.</span><span class="sxs-lookup"><span data-stu-id="6c655-186">Service remoting handles resolution of service addresses, connection, retry, and error handling.</span></span> <span data-ttu-id="6c655-187">Detta är tillgängligt för både C# och Java-program.</span><span class="sxs-lookup"><span data-stu-id="6c655-187">This is available for both C# and Java applications.</span></span>
* <span data-ttu-id="6c655-188">**HTTP**: för språkoberoende kommunikation HTTP innehåller en branschstandardiserad val med verktyg och HTTP-servrar som är tillgängliga på många olika språk, alla stöds inte av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6c655-188">**HTTP**: For language-agnostic communication, HTTP provides an industry-standard choice with tools and HTTP servers available in many different languages, all supported by Service Fabric.</span></span> <span data-ttu-id="6c655-189">Tjänster kan använda alla tillgängliga, till exempel HTTP stack [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) för C#-program.</span><span class="sxs-lookup"><span data-stu-id="6c655-189">Services can use any HTTP stack available, including [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) for C# applications.</span></span> <span data-ttu-id="6c655-190">Klienter som skrivits i C# kan utnyttja den `ICommunicationClient` och `ServicePartitionClient` klasser, medan Java, Använd den `CommunicationClient` och `FabricServicePartitionClient` klasser, [för tjänsten upplösning, HTTP-anslutningar och försök igen slingor](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="6c655-190">Clients written in C# can leverage the `ICommunicationClient` and `ServicePartitionClient` classes, whereas for Java, use the `CommunicationClient` and `FabricServicePartitionClient` classes, [for service resolution, HTTP connections, and retry loops](service-fabric-reliable-services-communication.md).</span></span>
* <span data-ttu-id="6c655-191">**WCF**: Om du har befintliga kod som använder WCF som din kommunikation framework, så du kan använda den `WcfCommunicationListener` för serversidan och `WcfCommunicationClient` och `ServicePartitionClient` klasser för klienten.</span><span class="sxs-lookup"><span data-stu-id="6c655-191">**WCF**: If you have existing code that uses WCF as your communication framework, then you can use the `WcfCommunicationListener` for the server side and `WcfCommunicationClient` and `ServicePartitionClient` classes for the client.</span></span> <span data-ttu-id="6c655-192">Detta men är bara tillgängligt för C#-program på Windows-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="6c655-192">This however is only available for C# applications on Windows based clusters.</span></span> <span data-ttu-id="6c655-193">Mer information finns i den här artikeln [WCF-baserad implementering av kommunikation stacken](service-fabric-reliable-services-communication-wcf.md).</span><span class="sxs-lookup"><span data-stu-id="6c655-193">For more details, see this article about [WCF-based implementation of the communication stack](service-fabric-reliable-services-communication-wcf.md).</span></span>

## <a name="using-custom-protocols-and-other-communication-frameworks"></a><span data-ttu-id="6c655-194">Med hjälp av anpassade protokoll och andra ramverk för kommunikation</span><span class="sxs-lookup"><span data-stu-id="6c655-194">Using custom protocols and other communication frameworks</span></span>
<span data-ttu-id="6c655-195">Tjänster kan använda alla protokoll eller ramverk för kommunikation, om det är ett anpassat protokoll för binära via TCP-sockets eller händelser via strömning via [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) eller [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="6c655-195">Services can use any protocol or framework for communication, whether its a custom binary protocol over TCP sockets, or streaming events through [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) or [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/).</span></span> <span data-ttu-id="6c655-196">Service Fabric ger kommunikation API: er som du ansluter din kommunikation stacken, medan allt arbete att upptäcka och ansluta representeras från dig.</span><span class="sxs-lookup"><span data-stu-id="6c655-196">Service Fabric provides communication APIs that you can plug your communication stack into, while all the work to discover and connect is abstracted from you.</span></span> <span data-ttu-id="6c655-197">Se den här artikeln om den [tillförlitlig kommunikation modell](service-fabric-reliable-services-communication.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6c655-197">See this article about the [Reliable Service communication model](service-fabric-reliable-services-communication.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c655-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c655-198">Next steps</span></span>
<span data-ttu-id="6c655-199">Mer information om begrepp och API: er som är tillgängliga i den [Reliable Services kommunikation modellen](service-fabric-reliable-services-communication.md), sedan komma igång snabbt med [remoting service](service-fabric-reliable-services-communication-remoting.md) eller gå på djupet att lära dig hur du skriver ett meddelande lyssnaren med [Web API med OWIN som värd för automatisk](service-fabric-reliable-services-communication-webapi.md).</span><span class="sxs-lookup"><span data-stu-id="6c655-199">Learn more about the concepts and APIs available in the [Reliable Services communication model](service-fabric-reliable-services-communication.md), then get started quickly with [service remoting](service-fabric-reliable-services-communication-remoting.md) or go in-depth to learn how to write a communication listener using [Web API with OWIN self-host](service-fabric-reliable-services-communication-webapi.md).</span></span>

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
[9]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[10]: ./media/service-fabric-reverseproxy/internal-communication.png
