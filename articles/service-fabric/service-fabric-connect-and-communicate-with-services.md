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
# <a name="connect-and-communicate-with-services-in-service-fabric"></a>Ansluta och kommunicera med tjänster i Service Fabric
I Service Fabric körs en tjänst någonstans i ett Service Fabric-kluster, vanligtvis fördelade på flera virtuella datorer. Den kan flyttas från en enda plats tooanother, antingen av hello tjänstens ägare eller automatiskt av Service Fabric. Tjänsterna är inte statiskt bundet tooa viss dator eller adress.

Ett Service Fabric-program består normalt av många olika tjänster, där varje tjänst utför en särskild aktivitet. Dessa tjänster kan kommunicera med varandra tooform en fullständig funktion, till exempel återgivning olika delar av ett webbprogram. Det finns också klienten program som ansluter tooand kommunicera med tjänster. Det här dokumentet beskrivs hur tooset upprätta kommunikation med och mellan dina tjänster i Service Fabric.

Den här Microsoft Virtual Academy videon beskrivs också kommunikation:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965">  
<img src="./media/service-fabric-connect-and-communicate-with-services/CommunicationVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="bring-your-own-protocol"></a>Ta med egna protokoll
Service Fabric hjälper dig att hantera hello livscykeln för dina tjänster, men det gör inte beslut om dina tjänster gör. Detta omfattar kommunikation. När tjänsten har öppnats av Service Fabric är din tjänst möjlighet tooset in en slutpunkt för inkommande begäranden du med oavsett protokoll eller kommunikation stack. Tjänsten kommer att lyssna på en normal **IP:port** med adresseringsschema, till exempel en URI-adress. Flera instanser av tjänsten eller repliker kan dela en värdprocess, då de kommer måste toouse olika portar eller använda en delning av port mekanism, till exempel hello http.sys kernel-drivrutinen i Windows. I båda fallen måste varje instans av tjänsten eller replik i en värdprocess måste vara unikt adresserbara.

![slutpunkter][1]

## <a name="service-discovery-and-resolution"></a>Identifiering och lösning
I ett distribuerat system, kan tjänster flytta från en dator tooanother över tid. Detta kan inträffa av olika skäl, inklusive resurser, uppgraderingar, redundans och skalbara. Det innebär att tjänsten slutpunkts-adresser ändras när hello tjänsten flyttar toonodes med olika IP-adresser och öppnas på olika portar om hello tjänsten använder en dynamiskt vald port.

![Distribution av tjänster][7]

Service Fabric ger en identifiering och lösning tjänsten anropade hello Naming Service. hello upprätthåller namngivningstjänst en tabell som mappar med namnet tjänstinstanser toohello slutpunkts-adresser som de lyssna på. Alla instanser för tjänsten i Service Fabric har unika namn som visas i form av URI: er, till exempel `"fabric:/MyApplication/MyService"`. hello namnet på hello tjänst ändras inte under hello livslängd för hello-tjänsten kan endast hello slutpunkts-adresser som kan ändras när tjänsterna flytta. Detta är detsamma toowebsites som har konstant URL: er men där hello IP-adress kan ändras. Och liknande tooDNS på hello web som matchar webbplatsadresser URL: er tooIP Service Fabric har ett register som mappar service namn tootheir slutpunktsadress.

![slutpunkter][2]

Lösa och ansluta tooservices omfattar hello följande köras i en slinga:

* **Lös**: Get hello slutpunkt som en tjänst har publicerat från hello Naming Service.
* **Ansluta**: ansluta toohello tjänsten via oavsett protokoll använder på denna slutpunkt.
* **Försök**: ett anslutningsförsök misslyckas av olika orsaker, till exempel om hello-tjänsten har flyttats sedan hello senaste gången hello slutpunktsadress löstes. I så fall hello föregående Lös och ansluta steg måste toobe igen och den här cykeln upprepas tills hello anslutningen lyckas.

## <a name="connecting-tooother-services"></a>Anslutningstjänsterna tooother
Tjänster som ansluter tooeach andra i ett kluster vanligtvis direkt åtkomst till hello slutpunkter av andra tjänster eftersom hello noder i ett kluster finns på hello samma lokala nätverk. toomake är enklare tooconnect mellan tjänster, Service Fabric ger ytterligare tjänster som använder hello Naming Service. En DNS-tjänst och en omvänd proxy-tjänst.


### <a name="dns-service"></a>DNS-tjänst
Eftersom många tjänster, särskilt av tjänster, kan ha ett befintligt URL-namn, som kan tooresolve dessa med hello standard DNS-protokollet (snarare än hello namngivningstjänst protocol) är praktiskt, särskilt i programmet ”lyfta och flytta” scenarier. Detta är exakt vilka hello DNS-tjänsten. Det gör du toomap DNS-namn tooa tjänstnamn och därför matcha IP-adresser för slutpunkt. 

Följande diagram, hello DNS-tjänsten körs i hello Service Fabric-klustret mappar som visas i hello DNS-namn tooservice namn som sedan kan matchas med hello namngivningstjänst tooreturn hello endpoint adresser tooconnect till. hello DNS-namn för hello tjänsten tillhandahålls för närvarande hello skapas. 

![slutpunkter][9]

Mer information om hur toouse hello DNS-tjänsten finns [DNS-tjänsten i Azure Service Fabric](service-fabric-dnsservice.md) artikel.

### <a name="reverse-proxy-service"></a>Tjänsten omvänd proxy
hello omvänd proxy adresser tjänster i hello-kluster som exponerar http-slutpunkter, inklusive HTTPS. hello omvänd proxy förenklar anropar andra tjänster och deras metoder genom att låta en specifik URI-format och hanterar hello lösa ansluta, försök steg som krävs för en tjänst toocommunicate med en annan med hello namngivning av tjänsten. Med andra ord döljer hello Naming Service från dig vid anrop av andra tjänster genom att göra detta så enkelt som anropar en URL.

![slutpunkter][10]

Mer information om hur toouse hello omvänd proxy-tjänsten finns [omvänd proxy i Azure Service Fabric](service-fabric-reverseproxy.md) artikel.

## <a name="connections-from-external-clients"></a>Anslutningar från externa klienter
Tjänster som ansluter tooeach andra i ett kluster vanligtvis direkt åtkomst till hello slutpunkter av andra tjänster eftersom hello noder i ett kluster finns på hello samma lokala nätverk. I vissa miljöer med kan ett kluster dock finnas bakom en belastningsutjämnare som skickar externa ingång trafik via en begränsad uppsättning portar. I dessa fall tjänster fortfarande kommunicera med varandra och Lös adresser med hjälp av hello Naming Service, men extra steg måste vidtas tooallow externa klienter tooconnect tooservices.

## <a name="service-fabric-in-azure"></a>Service Fabric i Azure
Ett Service Fabric-kluster i Azure är placerad bakom en belastningsutjämnare i Azure. Alla externa trafiken toohello klustret måste gå via hello belastningsutjämnaren. hello belastningsutjämnare automatiskt vidarebefordrar trafik inkommande på en slumpmässig porten tooa *nod* som har hello öppna samma port. hello Azure belastningsutjämnare bara medveten om portar som är öppna på hello *noder*, öppnar du portar genom att individuella inte känner *services*.

![Azure belastningsutjämnare och Service Fabric-topologi][3]

Till exempel i ordning tooaccept extern trafik på port **80**, måste vara konfigurerad hello följande saker:

1. Skriv en tjänst som lyssnar på port 80. Konfigurera port 80 i hello service ServiceManifest.xml och öppna en lyssnare i hello-tjänsten, till exempel en egen värdbaserade webbserver.

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
2. Skapa ett Service Fabric-kluster i Azure och ange port **80** som en anpassad endpoint-port för hello nodtypen som är värd för hello-tjänsten. Om du har mer än en nodtyp kan du skapa en *placering begränsningen* på hello tjänsten tooensure körs bara på hello nodtyp som har hello anpassade endpoint porten är öppen.

    ![Öppna en port på en nodtyp][4]
3. När hello klustret har skapats kan du konfigurera hello Azure belastningsutjämnare i hello klustrets resursgrupp tooforward trafik på port 80. När du skapar ett kluster via hello Azure-portalen kan ställs detta in automatiskt för varje anpassad endpoint-port som har konfigurerats.

    ![Vidarebefordra trafik i hello Azure belastningsutjämnare][5]
4. hello Azure belastningsutjämnare använder en avsökning toodetermine om eller inte toosend trafik tooa viss nod. hello avsökning regelbundet kontrollerar en slutpunkt på varje nod toodetermine huruvida hello nod svarar. Om hello avsökningen misslyckas tooreceive ett svar efter ett visst antal gånger stoppar hello belastningsutjämnaren skickar trafik toothat nod. När du skapar ett kluster via hello Azure-portalen, konfigureras automatiskt en avsökning för varje anpassad endpoint-port som har konfigurerats.

    ![Vidarebefordra trafik i hello Azure belastningsutjämnare][8]

Det är viktigt tooremember hello Azure belastningsutjämnare och hello avsökning bara känna till om hello *noder*, inte hello *services* körs på hello noder. hello Azure belastningsutjämnare kommer alltid att skicka trafik toonodes som svarar toohello avsökningen, så var försiktig tooensure tjänster är tillgängliga på hello-noder som kan toorespond toohello avsökning.

## <a name="reliable-services-built-in-communication-api-options"></a>Reliable Services: Inbyggda API kommunikationsalternativ
hello Reliable Services framework levereras med flera fördefinierade kommunikationsalternativ. hello beslutet om vilka en fungerar bäst för dig beror på hello val av hello programming modellen och hello kommunikation framework hello programmeringsspråket som dina tjänster har skrivits i.

* **Inget specifikt protokoll:** om du inte har ett visst val av kommunikation framework, men du vill tooget något igång snabbt och sedan hello perfekt alternativ för du är [remoting service](service-fabric-reliable-services-communication-remoting.md), vilket gör att strikt typkontroll RPC-anrop för Reliable Services och Reliable Actors. Detta är hello enklaste och snabbaste sättet tooget igång med service-kommunikation. Tjänsten fjärrkommunikation hanterar lösning av postadresser, anslutning, försök igen och felhantering. Detta är tillgängligt för både C# och Java-program.
* **HTTP**: för språkoberoende kommunikation HTTP innehåller en branschstandardiserad val med verktyg och HTTP-servrar som är tillgängliga på många olika språk, alla stöds inte av Service Fabric. Tjänster kan använda alla tillgängliga, till exempel HTTP stack [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) för C#-program. Klienter som skrivits i C# kan utnyttja hello `ICommunicationClient` och `ServicePartitionClient` klasser, medan för Java, använda hello `CommunicationClient` och `FabricServicePartitionClient` klasser, [för tjänsten upplösning, HTTP-anslutningar och försök igen slingor](service-fabric-reliable-services-communication.md).
* **WCF**: Om du har befintliga kod som använder WCF som din kommunikation framework, så du kan använda hello `WcfCommunicationListener` för hello på serversidan och `WcfCommunicationClient` och `ServicePartitionClient` klasser för hello-klienten. Detta men är bara tillgängligt för C#-program på Windows-baserade kluster. Mer information finns i den här artikeln [WCF-baserad implementering av hello kommunikation stack](service-fabric-reliable-services-communication-wcf.md).

## <a name="using-custom-protocols-and-other-communication-frameworks"></a>Med hjälp av anpassade protokoll och andra ramverk för kommunikation
Tjänster kan använda alla protokoll eller ramverk för kommunikation, om det är ett anpassat protokoll för binära via TCP-sockets eller händelser via strömning via [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) eller [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/). Service Fabric ger kommunikation API: er som du ansluter din kommunikation stacken, medan alla hello arbetar toodiscover och ansluta representeras från dig. Finns den här artikeln om hello [tillförlitlig kommunikation modell](service-fabric-reliable-services-communication.md) för mer information.

## <a name="next-steps"></a>Nästa steg
Lär dig mer om hello begrepp och API: er som är tillgängliga i hello [Reliable Services kommunikation modellen](service-fabric-reliable-services-communication.md), sedan komma igång snabbt med [remoting service](service-fabric-reliable-services-communication-remoting.md) eller gå djupgående toolearn hur toowrite en lyssnare för kommunikation med [Web API med OWIN som värd för automatisk](service-fabric-reliable-services-communication-webapi.md).

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
[9]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[10]: ./media/service-fabric-reverseproxy/internal-communication.png
