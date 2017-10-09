---
title: "aaaConnect och kommunicera med tjänster i Azure Service Fabric | Microsoft Docs"
description: "Lär dig hur tooresolve, ansluta och kommunicera med tjänster i Service Fabric."
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
ms.openlocfilehash: b8b374a71d4c5d21f48a560a3a8c81b357fe418d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-and-communicate-with-services-in-service-fabric"></a><span data-ttu-id="e3a38-103">Ansluta och kommunicera med tjänster i Service Fabric</span><span class="sxs-lookup"><span data-stu-id="e3a38-103">Connect and communicate with services in Service Fabric</span></span>
<span data-ttu-id="e3a38-104">I Service Fabric körs en tjänst någonstans i ett Service Fabric-kluster, vanligtvis fördelade på flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e3a38-104">In Service Fabric, a service runs somewhere in a Service Fabric cluster, typically distributed across multiple VMs.</span></span> <span data-ttu-id="e3a38-105">Den kan flyttas från en enda plats tooanother, antingen av hello tjänstens ägare eller automatiskt av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="e3a38-105">It can be moved from one place tooanother, either by hello service owner, or automatically by Service Fabric.</span></span> <span data-ttu-id="e3a38-106">Tjänsterna är inte statiskt bundet tooa viss dator eller adress.</span><span class="sxs-lookup"><span data-stu-id="e3a38-106">Services are not statically tied tooa particular machine or address.</span></span>

<span data-ttu-id="e3a38-107">Ett Service Fabric-program består normalt av många olika tjänster, där varje tjänst utför en särskild aktivitet.</span><span class="sxs-lookup"><span data-stu-id="e3a38-107">A Service Fabric application is generally composed of many different services, where each service performs a specialized task.</span></span> <span data-ttu-id="e3a38-108">Dessa tjänster kan kommunicera med varandra tooform en fullständig funktion, till exempel återgivning olika delar av ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="e3a38-108">These services may communicate with each other tooform a complete function, such as rendering different parts of a web application.</span></span> <span data-ttu-id="e3a38-109">Det finns också klienten program som ansluter tooand kommunicera med tjänster.</span><span class="sxs-lookup"><span data-stu-id="e3a38-109">There are also client applications that connect tooand communicate with services.</span></span> <span data-ttu-id="e3a38-110">Det här dokumentet beskrivs hur tooset upprätta kommunikation med och mellan dina tjänster i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="e3a38-110">This document discusses how tooset up communication with and between your services in Service Fabric.</span></span>

<span data-ttu-id="e3a38-111">Den här Microsoft Virtual Academy videon beskrivs också kommunikation:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965"></span><span class="sxs-lookup"><span data-stu-id="e3a38-111">This Microsoft Virtual Academy video also discusses service communication: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965"></span></span>  
<img src="./media/service-fabric-connect-and-communicate-with-services/CommunicationVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="bring-your-own-protocol"></a><span data-ttu-id="e3a38-112">Ta med egna protokoll</span><span class="sxs-lookup"><span data-stu-id="e3a38-112">Bring your own protocol</span></span>
<span data-ttu-id="e3a38-113">Service Fabric hjälper dig att hantera hello livscykeln för dina tjänster, men det gör inte beslut om dina tjänster gör.</span><span class="sxs-lookup"><span data-stu-id="e3a38-113">Service Fabric helps manage hello lifecycle of your services but it does not make decisions about what your services do.</span></span> <span data-ttu-id="e3a38-114">Detta omfattar kommunikation.</span><span class="sxs-lookup"><span data-stu-id="e3a38-114">This includes communication.</span></span> <span data-ttu-id="e3a38-115">När tjänsten har öppnats av Service Fabric är din tjänst möjlighet tooset in en slutpunkt för inkommande begäranden du med oavsett protokoll eller kommunikation stack.</span><span class="sxs-lookup"><span data-stu-id="e3a38-115">When your service is opened by Service Fabric, that's your service's opportunity tooset up an endpoint for incoming requests, using whatever protocol or communication stack you want.</span></span> <span data-ttu-id="e3a38-116">Tjänsten kommer att lyssna på en normal **IP:port** med adresseringsschema, till exempel en URI-adress.</span><span class="sxs-lookup"><span data-stu-id="e3a38-116">Your service will listen on a normal **IP:port** address using any addressing scheme, such as a URI.</span></span> <span data-ttu-id="e3a38-117">Flera instanser av tjänsten eller repliker kan dela en värdprocess, då de kommer måste toouse olika portar eller använda en delning av port mekanism, till exempel hello http.sys kernel-drivrutinen i Windows.</span><span class="sxs-lookup"><span data-stu-id="e3a38-117">Multiple service instances or replicas may share a host process, in which case they will either need toouse different ports or use a port-sharing mechanism, such as hello http.sys kernel driver in Windows.</span></span> <span data-ttu-id="e3a38-118">I båda fallen måste varje instans av tjänsten eller replik i en värdprocess måste vara unikt adresserbara.</span><span class="sxs-lookup"><span data-stu-id="e3a38-118">In either case, each service instance or replica in a host process must be uniquely addressable.</span></span>

![slutpunkter][1]

## <a name="service-discovery-and-resolution"></a><span data-ttu-id="e3a38-120">Identifiering och lösning</span><span class="sxs-lookup"><span data-stu-id="e3a38-120">Service discovery and resolution</span></span>
<span data-ttu-id="e3a38-121">I ett distribuerat system, kan tjänster flytta från en dator tooanother över tid.</span><span class="sxs-lookup"><span data-stu-id="e3a38-121">In a distributed system, services may move from one machine tooanother over time.</span></span> <span data-ttu-id="e3a38-122">Detta kan inträffa av olika skäl, inklusive resurser, uppgraderingar, redundans och skalbara. Det innebär att tjänsten slutpunkts-adresser ändras när hello tjänsten flyttar toonodes med olika IP-adresser och öppnas på olika portar om hello tjänsten använder en dynamiskt vald port.</span><span class="sxs-lookup"><span data-stu-id="e3a38-122">This can happen for various reasons, including resource balancing, upgrades, failovers, or scale-out. This means service endpoint addresses change as hello service moves toonodes with different IP addresses, and may open on different ports if hello service uses a dynamically selected port.</span></span>

![Distribution av tjänster][7]

<span data-ttu-id="e3a38-124">Service Fabric ger en identifiering och lösning tjänsten anropade hello Naming Service.</span><span class="sxs-lookup"><span data-stu-id="e3a38-124">Service Fabric provides a discovery and resolution service called hello Naming Service.</span></span> <span data-ttu-id="e3a38-125">hello upprätthåller namngivningstjänst en tabell som mappar med namnet tjänstinstanser toohello slutpunkts-adresser som de lyssna på.</span><span class="sxs-lookup"><span data-stu-id="e3a38-125">hello Naming Service maintains a table that maps named service instances toohello endpoint addresses they listen on.</span></span> <span data-ttu-id="e3a38-126">Alla instanser för tjänsten i Service Fabric har unika namn som visas i form av URI: er, till exempel `"fabric:/MyApplication/MyService"`.</span><span class="sxs-lookup"><span data-stu-id="e3a38-126">All named service instances in Service Fabric have unique names represented as URIs, for example, `"fabric:/MyApplication/MyService"`.</span></span> <span data-ttu-id="e3a38-127">hello namnet på hello tjänst ändras inte under hello livslängd för hello-tjänsten kan endast hello slutpunkts-adresser som kan ändras när tjänsterna flytta.</span><span class="sxs-lookup"><span data-stu-id="e3a38-127">hello name of hello service does not change over hello lifetime of hello service, it's only hello endpoint addresses that can change when services move.</span></span> <span data-ttu-id="e3a38-128">Detta är detsamma toowebsites som har konstant URL: er men där hello IP-adress kan ändras.</span><span class="sxs-lookup"><span data-stu-id="e3a38-128">This is analogous toowebsites that have constant URLs but where hello IP address may change.</span></span> <span data-ttu-id="e3a38-129">Och liknande tooDNS på hello web som matchar webbplatsadresser URL: er tooIP Service Fabric har ett register som mappar service namn tootheir slutpunktsadress.</span><span class="sxs-lookup"><span data-stu-id="e3a38-129">And similar tooDNS on hello web, which resolves website URLs tooIP addresses, Service Fabric has a registrar that maps service names tootheir endpoint address.</span></span>

![slutpunkter][2]

<span data-ttu-id="e3a38-131">Lösa och ansluta tooservices omfattar hello följande köras i en slinga:</span><span class="sxs-lookup"><span data-stu-id="e3a38-131">Resolving and connecting tooservices involves hello following steps run in a loop:</span></span>

* <span data-ttu-id="e3a38-132">**Lös**: Get hello slutpunkt som en tjänst har publicerat från hello Naming Service.</span><span class="sxs-lookup"><span data-stu-id="e3a38-132">**Resolve**: Get hello endpoint that a service has published from hello Naming Service.</span></span>
* <span data-ttu-id="e3a38-133">**Ansluta**: ansluta toohello tjänsten via oavsett protokoll använder på denna slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="e3a38-133">**Connect**: Connect toohello service over whatever protocol it uses on that endpoint.</span></span>
* <span data-ttu-id="e3a38-134">**Försök**: ett anslutningsförsök misslyckas av olika orsaker, till exempel om hello-tjänsten har flyttats sedan hello senaste gången hello slutpunktsadress löstes.</span><span class="sxs-lookup"><span data-stu-id="e3a38-134">**Retry**: A connection attempt may fail for any number of reasons, for example if hello service has moved since hello last time hello endpoint address was resolved.</span></span> <span data-ttu-id="e3a38-135">I så fall hello föregående Lös och ansluta steg måste toobe igen och den här cykeln upprepas tills hello anslutningen lyckas.</span><span class="sxs-lookup"><span data-stu-id="e3a38-135">In that case, hello preceding resolve and connect steps need toobe retried, and this cycle is repeated until hello connection succeeds.</span></span>

## <a name="connecting-tooother-services"></a><span data-ttu-id="e3a38-136">Anslutningstjänsterna tooother</span><span class="sxs-lookup"><span data-stu-id="e3a38-136">Connecting tooother services</span></span>
<span data-ttu-id="e3a38-137">Tjänster som ansluter tooeach andra i ett kluster vanligtvis direkt åtkomst till hello slutpunkter av andra tjänster eftersom hello noder i ett kluster finns på hello samma lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="e3a38-137">Services connecting tooeach other inside a cluster generally can directly access hello endpoints of other services because hello nodes in a cluster are on hello same local network.</span></span> <span data-ttu-id="e3a38-138">toomake är enklare tooconnect mellan tjänster, Service Fabric ger ytterligare tjänster som använder hello Naming Service.</span><span class="sxs-lookup"><span data-stu-id="e3a38-138">toomake is easier tooconnect between services, Service Fabric provides additional services that use hello Naming Service.</span></span> <span data-ttu-id="e3a38-139">En DNS-tjänst och en omvänd proxy-tjänst.</span><span class="sxs-lookup"><span data-stu-id="e3a38-139">A DNS service and a reverse proxy service.</span></span>


### <a name="dns-service"></a><span data-ttu-id="e3a38-140">DNS-tjänst</span><span class="sxs-lookup"><span data-stu-id="e3a38-140">DNS service</span></span>
<span data-ttu-id="e3a38-141">Eftersom många tjänster, särskilt av tjänster, kan ha ett befintligt URL-namn, som kan tooresolve dessa med hello standard DNS-protokollet (snarare än hello namngivningstjänst protocol) är praktiskt, särskilt i programmet ”lyfta och flytta” scenarier.</span><span class="sxs-lookup"><span data-stu-id="e3a38-141">Since many services, especially containerized services, can have an existing URL name, being able tooresolve these using hello standard DNS protocol (rather than hello Naming Service protocol) is very convenient, especially in application "lift and shift" scenarios.</span></span> <span data-ttu-id="e3a38-142">Detta är exakt vilka hello DNS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e3a38-142">This is exactly what hello DNS service does.</span></span> <span data-ttu-id="e3a38-143">Det gör du toomap DNS-namn tooa tjänstnamn och därför matcha IP-adresser för slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="e3a38-143">It enables you toomap DNS names tooa service name and hence resolve endpoint IP addresses.</span></span> 

<span data-ttu-id="e3a38-144">Följande diagram, hello DNS-tjänsten körs i hello Service Fabric-klustret mappar som visas i hello DNS-namn tooservice namn som sedan kan matchas med hello namngivningstjänst tooreturn hello endpoint adresser tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="e3a38-144">As shown in hello following diagram, hello DNS service, running in hello Service Fabric cluster, maps DNS names tooservice names which are then resolved by hello Naming Service tooreturn hello endpoint addresses tooconnect to.</span></span> <span data-ttu-id="e3a38-145">hello DNS-namn för hello tjänsten tillhandahålls för närvarande hello skapas.</span><span class="sxs-lookup"><span data-stu-id="e3a38-145">hello DNS name for hello service is provided at hello time of creation.</span></span> 

![slutpunkter][9]

<span data-ttu-id="e3a38-147">Mer information om hur toouse hello DNS-tjänsten finns [DNS-tjänsten i Azure Service Fabric](service-fabric-dnsservice.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="e3a38-147">For more details on how toouse hello DNS service see [DNS service in Azure Service Fabric](service-fabric-dnsservice.md) article.</span></span>

### <a name="reverse-proxy-service"></a><span data-ttu-id="e3a38-148">Tjänsten omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="e3a38-148">Reverse proxy service</span></span>
<span data-ttu-id="e3a38-149">hello omvänd proxy adresser tjänster i hello-kluster som exponerar http-slutpunkter, inklusive HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e3a38-149">hello reverse proxy addresses services in hello cluster that exposes HTTP endpoints including HTTPS.</span></span> <span data-ttu-id="e3a38-150">hello omvänd proxy förenklar anropar andra tjänster och deras metoder genom att låta en specifik URI-format och hanterar hello lösa ansluta, försök steg som krävs för en tjänst toocommunicate med en annan med hello namngivning av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e3a38-150">hello reverse proxy greatly simplifies calling other services and their methods by having a specific URI format and handles hello resolve, connect, retry steps required for one service toocommunicate with another using hello Naming Serivce.</span></span> <span data-ttu-id="e3a38-151">Med andra ord döljer hello Naming Service från dig vid anrop av andra tjänster genom att göra detta så enkelt som anropar en URL.</span><span class="sxs-lookup"><span data-stu-id="e3a38-151">In other words, it hides hello Naming Service from you when calling other services by making this as simple as calling a URL.</span></span>

![slutpunkter][10]

<span data-ttu-id="e3a38-153">Mer information om hur toouse hello omvänd proxy-tjänsten finns [omvänd proxy i Azure Service Fabric](service-fabric-reverseproxy.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="e3a38-153">For more details on how toouse hello reverse proxy service see [Reverse proxy in Azure Service Fabric](service-fabric-reverseproxy.md) article.</span></span>

## <a name="connections-from-external-clients"></a><span data-ttu-id="e3a38-154">Anslutningar från externa klienter</span><span class="sxs-lookup"><span data-stu-id="e3a38-154">Connections from external clients</span></span>
<span data-ttu-id="e3a38-155">Tjänster som ansluter tooeach andra i ett kluster vanligtvis direkt åtkomst till hello slutpunkter av andra tjänster eftersom hello noder i ett kluster finns på hello samma lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="e3a38-155">Services connecting tooeach other inside a cluster generally can directly access hello endpoints of other services because hello nodes in a cluster are on hello same local network.</span></span> <span data-ttu-id="e3a38-156">I vissa miljöer med kan ett kluster dock finnas bakom en belastningsutjämnare som skickar externa ingång trafik via en begränsad uppsättning portar.</span><span class="sxs-lookup"><span data-stu-id="e3a38-156">In some environments, however, a cluster may be behind a load balancer that routes external ingress traffic through a limited set of ports.</span></span> <span data-ttu-id="e3a38-157">I dessa fall tjänster fortfarande kommunicera med varandra och Lös adresser med hjälp av hello Naming Service, men extra steg måste vidtas tooallow externa klienter tooconnect tooservices.</span><span class="sxs-lookup"><span data-stu-id="e3a38-157">In these cases, services can still communicate with each other and resolve addresses using hello Naming Service, but extra steps must be taken tooallow external clients tooconnect tooservices.</span></span>

## <a name="service-fabric-in-azure"></a><span data-ttu-id="e3a38-158">Service Fabric i Azure</span><span class="sxs-lookup"><span data-stu-id="e3a38-158">Service Fabric in Azure</span></span>
<span data-ttu-id="e3a38-159">Ett Service Fabric-kluster i Azure är placerad bakom en belastningsutjämnare i Azure.</span><span class="sxs-lookup"><span data-stu-id="e3a38-159">A Service Fabric cluster in Azure is placed behind an Azure Load Balancer.</span></span> <span data-ttu-id="e3a38-160">Alla externa trafiken toohello klustret måste gå via hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="e3a38-160">All external traffic toohello cluster must pass through hello load balancer.</span></span> <span data-ttu-id="e3a38-161">hello belastningsutjämnare automatiskt vidarebefordrar trafik inkommande på en slumpmässig porten tooa *nod* som har hello öppna samma port.</span><span class="sxs-lookup"><span data-stu-id="e3a38-161">hello load balancer will automatically forward traffic inbound on a given port tooa random *node* that has hello same port open.</span></span> <span data-ttu-id="e3a38-162">hello Azure belastningsutjämnare bara medveten om portar som är öppna på hello *noder*, öppnar du portar genom att individuella inte känner *services*.</span><span class="sxs-lookup"><span data-stu-id="e3a38-162">hello Azure Load Balancer only knows about ports open on hello *nodes*, it does not know about ports open by individual *services*.</span></span>

![Azure belastningsutjämnare och Service Fabric-topologi][3]

<span data-ttu-id="e3a38-164">Till exempel i ordning tooaccept extern trafik på port **80**, måste vara konfigurerad hello följande saker:</span><span class="sxs-lookup"><span data-stu-id="e3a38-164">For example, in order tooaccept external traffic on port **80**, hello following things must be configured:</span></span>

1. <span data-ttu-id="e3a38-165">Skriv en tjänst som lyssnar på port 80.</span><span class="sxs-lookup"><span data-stu-id="e3a38-165">Write a service that listens on port 80.</span></span> <span data-ttu-id="e3a38-166">Konfigurera port 80 i hello service ServiceManifest.xml och öppna en lyssnare i hello-tjänsten, till exempel en egen värdbaserade webbserver.</span><span class="sxs-lookup"><span data-stu-id="e3a38-166">Configure port 80 in hello service's ServiceManifest.xml and open a listener in hello service, for example, a self-hosted web server.</span></span>

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
2. <span data-ttu-id="e3a38-167">Skapa ett Service Fabric-kluster i Azure och ange port **80** som en anpassad endpoint-port för hello nodtypen som är värd för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e3a38-167">Create a Service Fabric Cluster in Azure and specify port **80** as a custom endpoint port for hello node type that will host hello service.</span></span> <span data-ttu-id="e3a38-168">Om du har mer än en nodtyp kan du skapa en *placering begränsningen* på hello tjänsten tooensure körs bara på hello nodtyp som har hello anpassade endpoint porten är öppen.</span><span class="sxs-lookup"><span data-stu-id="e3a38-168">If you have more than one node type, you can set up a *placement constraint* on hello service tooensure it only runs on hello node type that has hello custom endpoint port opened.</span></span>

    ![Öppna en port på en nodtyp][4]
3. <span data-ttu-id="e3a38-170">När hello klustret har skapats kan du konfigurera hello Azure belastningsutjämnare i hello klustrets resursgrupp tooforward trafik på port 80.</span><span class="sxs-lookup"><span data-stu-id="e3a38-170">Once hello cluster has been created, configure hello Azure Load Balancer in hello cluster's Resource Group tooforward traffic on port 80.</span></span> <span data-ttu-id="e3a38-171">När du skapar ett kluster via hello Azure-portalen kan ställs detta in automatiskt för varje anpassad endpoint-port som har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="e3a38-171">When creating a cluster through hello Azure portal, this is set up automatically for each custom endpoint port that was configured.</span></span>

    ![Vidarebefordra trafik i hello Azure belastningsutjämnare][5]
4. <span data-ttu-id="e3a38-173">hello Azure belastningsutjämnare använder en avsökning toodetermine om eller inte toosend trafik tooa viss nod.</span><span class="sxs-lookup"><span data-stu-id="e3a38-173">hello Azure Load Balancer uses a probe toodetermine whether or not toosend traffic tooa particular node.</span></span> <span data-ttu-id="e3a38-174">hello avsökning regelbundet kontrollerar en slutpunkt på varje nod toodetermine huruvida hello nod svarar.</span><span class="sxs-lookup"><span data-stu-id="e3a38-174">hello probe periodically checks an endpoint on each node toodetermine whether or not hello node is responding.</span></span> <span data-ttu-id="e3a38-175">Om hello avsökningen misslyckas tooreceive ett svar efter ett visst antal gånger stoppar hello belastningsutjämnaren skickar trafik toothat nod.</span><span class="sxs-lookup"><span data-stu-id="e3a38-175">If hello probe fails tooreceive a response after a configured number of times, hello load balancer stops sending traffic toothat node.</span></span> <span data-ttu-id="e3a38-176">När du skapar ett kluster via hello Azure-portalen, konfigureras automatiskt en avsökning för varje anpassad endpoint-port som har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="e3a38-176">When creating a cluster through hello Azure portal, a probe is automatically set up for each custom endpoint port that was configured.</span></span>

    ![Vidarebefordra trafik i hello Azure belastningsutjämnare][8]

<span data-ttu-id="e3a38-178">Det är viktigt tooremember hello Azure belastningsutjämnare och hello avsökning bara känna till om hello *noder*, inte hello *services* körs på hello noder.</span><span class="sxs-lookup"><span data-stu-id="e3a38-178">It's important tooremember that hello Azure Load Balancer and hello probe only know about hello *nodes*, not hello *services* running on hello nodes.</span></span> <span data-ttu-id="e3a38-179">hello Azure belastningsutjämnare kommer alltid att skicka trafik toonodes som svarar toohello avsökningen, så var försiktig tooensure tjänster är tillgängliga på hello-noder som kan toorespond toohello avsökning.</span><span class="sxs-lookup"><span data-stu-id="e3a38-179">hello Azure Load Balancer will always send traffic toonodes that respond toohello probe, so care must be taken tooensure services are available on hello nodes that are able toorespond toohello probe.</span></span>

## <a name="reliable-services-built-in-communication-api-options"></a><span data-ttu-id="e3a38-180">Reliable Services: Inbyggda API kommunikationsalternativ</span><span class="sxs-lookup"><span data-stu-id="e3a38-180">Reliable Services: Built-in communication API options</span></span>
<span data-ttu-id="e3a38-181">hello Reliable Services framework levereras med flera fördefinierade kommunikationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="e3a38-181">hello Reliable Services framework ships with several pre-built communication options.</span></span> <span data-ttu-id="e3a38-182">hello beslutet om vilka en fungerar bäst för dig beror på hello val av hello programming modellen och hello kommunikation framework hello programmeringsspråket som dina tjänster har skrivits i.</span><span class="sxs-lookup"><span data-stu-id="e3a38-182">hello decision about which one will work best for you depends on hello choice of hello programming model, hello communication framework, and hello programming language that your services are written in.</span></span>

* <span data-ttu-id="e3a38-183">**Inget specifikt protokoll:** om du inte har ett visst val av kommunikation framework, men du vill tooget något igång snabbt och sedan hello perfekt alternativ för du är [remoting service](service-fabric-reliable-services-communication-remoting.md), vilket gör att strikt typkontroll RPC-anrop för Reliable Services och Reliable Actors.</span><span class="sxs-lookup"><span data-stu-id="e3a38-183">**No specific protocol:**  If you don't have a particular choice of communication framework, but you want tooget something up and running quickly, then hello ideal option for you is [service remoting](service-fabric-reliable-services-communication-remoting.md), which allows strongly-typed remote procedure calls for Reliable Services and Reliable Actors.</span></span> <span data-ttu-id="e3a38-184">Detta är hello enklaste och snabbaste sättet tooget igång med service-kommunikation.</span><span class="sxs-lookup"><span data-stu-id="e3a38-184">This is hello easiest and fastest way tooget started with service communication.</span></span> <span data-ttu-id="e3a38-185">Tjänsten fjärrkommunikation hanterar lösning av postadresser, anslutning, försök igen och felhantering.</span><span class="sxs-lookup"><span data-stu-id="e3a38-185">Service remoting handles resolution of service addresses, connection, retry, and error handling.</span></span> <span data-ttu-id="e3a38-186">Detta är tillgängligt för både C# och Java-program.</span><span class="sxs-lookup"><span data-stu-id="e3a38-186">This is available for both C# and Java applications.</span></span>
* <span data-ttu-id="e3a38-187">**HTTP**: för språkoberoende kommunikation HTTP innehåller en branschstandardiserad val med verktyg och HTTP-servrar som är tillgängliga på många olika språk, alla stöds inte av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="e3a38-187">**HTTP**: For language-agnostic communication, HTTP provides an industry-standard choice with tools and HTTP servers available in many different languages, all supported by Service Fabric.</span></span> <span data-ttu-id="e3a38-188">Tjänster kan använda alla tillgängliga, till exempel HTTP stack [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) för C#-program.</span><span class="sxs-lookup"><span data-stu-id="e3a38-188">Services can use any HTTP stack available, including [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) for C# applications.</span></span> <span data-ttu-id="e3a38-189">Klienter som skrivits i C# kan utnyttja hello `ICommunicationClient` och `ServicePartitionClient` klasser, medan för Java, använda hello `CommunicationClient` och `FabricServicePartitionClient` klasser, [för tjänsten upplösning, HTTP-anslutningar och försök igen slingor](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="e3a38-189">Clients written in C# can leverage hello `ICommunicationClient` and `ServicePartitionClient` classes, whereas for Java, use hello `CommunicationClient` and `FabricServicePartitionClient` classes, [for service resolution, HTTP connections, and retry loops](service-fabric-reliable-services-communication.md).</span></span>
* <span data-ttu-id="e3a38-190">**WCF**: Om du har befintliga kod som använder WCF som din kommunikation framework, så du kan använda hello `WcfCommunicationListener` för hello på serversidan och `WcfCommunicationClient` och `ServicePartitionClient` klasser för hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="e3a38-190">**WCF**: If you have existing code that uses WCF as your communication framework, then you can use hello `WcfCommunicationListener` for hello server side and `WcfCommunicationClient` and `ServicePartitionClient` classes for hello client.</span></span> <span data-ttu-id="e3a38-191">Detta men är bara tillgängligt för C#-program på Windows-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="e3a38-191">This however is only available for C# applications on Windows based clusters.</span></span> <span data-ttu-id="e3a38-192">Mer information finns i den här artikeln [WCF-baserad implementering av hello kommunikation stack](service-fabric-reliable-services-communication-wcf.md).</span><span class="sxs-lookup"><span data-stu-id="e3a38-192">For more details, see this article about [WCF-based implementation of hello communication stack](service-fabric-reliable-services-communication-wcf.md).</span></span>

## <a name="using-custom-protocols-and-other-communication-frameworks"></a><span data-ttu-id="e3a38-193">Med hjälp av anpassade protokoll och andra ramverk för kommunikation</span><span class="sxs-lookup"><span data-stu-id="e3a38-193">Using custom protocols and other communication frameworks</span></span>
<span data-ttu-id="e3a38-194">Tjänster kan använda alla protokoll eller ramverk för kommunikation, om det är ett anpassat protokoll för binära via TCP-sockets eller händelser via strömning via [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) eller [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="e3a38-194">Services can use any protocol or framework for communication, whether its a custom binary protocol over TCP sockets, or streaming events through [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) or [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/).</span></span> <span data-ttu-id="e3a38-195">Service Fabric ger kommunikation API: er som du ansluter din kommunikation stacken, medan alla hello arbetar toodiscover och ansluta representeras från dig.</span><span class="sxs-lookup"><span data-stu-id="e3a38-195">Service Fabric provides communication APIs that you can plug your communication stack into, while all hello work toodiscover and connect is abstracted from you.</span></span> <span data-ttu-id="e3a38-196">Finns den här artikeln om hello [tillförlitlig kommunikation modell](service-fabric-reliable-services-communication.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="e3a38-196">See this article about hello [Reliable Service communication model](service-fabric-reliable-services-communication.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e3a38-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e3a38-197">Next steps</span></span>
<span data-ttu-id="e3a38-198">Lär dig mer om hello begrepp och API: er som är tillgängliga i hello [Reliable Services kommunikation modellen](service-fabric-reliable-services-communication.md), sedan komma igång snabbt med [remoting service](service-fabric-reliable-services-communication-remoting.md) eller gå djupgående toolearn hur toowrite en lyssnare för kommunikation med [Web API med OWIN som värd för automatisk](service-fabric-reliable-services-communication-webapi.md).</span><span class="sxs-lookup"><span data-stu-id="e3a38-198">Learn more about hello concepts and APIs available in hello [Reliable Services communication model](service-fabric-reliable-services-communication.md), then get started quickly with [service remoting](service-fabric-reliable-services-communication-remoting.md) or go in-depth toolearn how toowrite a communication listener using [Web API with OWIN self-host](service-fabric-reliable-services-communication-webapi.md).</span></span>

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
[9]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[10]: ./media/service-fabric-reverseproxy/internal-communication.png
