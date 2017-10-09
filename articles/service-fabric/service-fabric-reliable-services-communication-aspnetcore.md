---
title: aaaService kommunikation med hello ASP.NET Core | Microsoft Docs
description: "Lär dig hur toouse ASP.NET Core i tillståndslösa och tillståndskänsliga Reliable Services."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 8aa4668d-cbb6-4225-bd2d-ab5925a868f2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 05/02/2017
ms.author: vturecek
ms.openlocfilehash: 6e6a83ab04390150292f63de5d9b51d290284e50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-core-in-service-fabric-reliable-services"></a>ASP.NET Core i Service Fabric Reliable Services

ASP.NET Core är ett nytt öppen källkod och plattformsoberoende ramverk för att bygga moderna molnbaserade Internetanslutna program, till exempel webbappar, IoT-appar och mobila serverdelar. 

Den här artikeln är en djupgående guide toohosting ASP.NET Core services i Service Fabric Reliable Services med hello **Microsoft.ServiceFabric.AspNetCore.** * uppsättning NuGet-paket.

En inledande självstudiekurs om ASP.NET Core i Service Fabric och anvisningar om hur din utvecklingsmiljö konfigurera finns i [bygga en webbklientdel för ditt program med hjälp av ASP.NET Core](service-fabric-add-a-web-frontend.md).

hello resten av den här artikeln förutsätter att du redan är bekant med ASP.NET Core. Om inte, vi rekommenderar att läsa igenom hello [ASP.NET grunderna](https://docs.microsoft.com/aspnet/core/fundamentals/index).

## <a name="aspnet-core-in-hello-service-fabric-environment"></a>ASP.NET Core i hello Service Fabric-miljö

När ASP.NET Core appar kan köras på .NET Core eller hello fullständig .NET Framework, Service Fabric-tjänster för närvarande kan endast köras på hello fullständig .NET Framework. Det innebär när du skapar en ASP.NET Core Service Fabric-tjänst, du måste fortfarande använda hello fullständig .NET Framework.

ASP.NET Core kan användas på två olika sätt i Service Fabric:
 - **Värdbaserad som en gäst körbar fil**. Detta är främst används toorun befintliga ASP.NET Core program på Service Fabric utan ändringar i koden.
 - **Körs i en tillförlitlig tjänst**. Detta kan bättre integration med hello Service Fabric runtime och tillståndskänsliga ASP.NET Core services.

hello resten av den här artikeln förklarar hur toouse ASP.NET Core inuti en tillförlitlig tjänst med hjälp av hello ASP.NET Core integrationskomponenterna som levereras med hello Service Fabric-SDK. 

## <a name="service-fabric-service-hosting"></a>Tjänsten värd för Service Fabric

I Service Fabric en eller flera instanser och/eller repliker av din tjänst köras i en *tjänsten värdprocess*, en körbar fil som körs koden för tjänsten. Du, som en tjänst författare äger hello serverprocess och Service Fabric aktiveras och övervakar du.

Traditionella ASP.NET (upp tooMVC 5) är tätt kopplade tooIIS via System.Web.dll. ASP.NET Core ger en åtskillnad mellan hello webbserver och ditt webbprogram. Detta gör web applications toobe bärbar mellan olika webbservrar och kan också web servrar toobe *egenvärdbaserat*, vilket innebär att du kan starta en webbserver i en egen process som skillnad från tooa process som ägs av dedicerad serverprogram, till exempel IIS. 

Du måste vara kan toostart ASP.NET i din serverprocess i ordning toocombine Service Fabric-tjänsten och ASP.NET, antingen som en gäst körbar fil eller i en tillförlitlig tjänst. ASP.NET Core värd själv kan du toodo detta.

## <a name="hosting-aspnet-core-in-a-reliable-service"></a>Värd för ASP.NET Core i en tillförlitlig tjänst
Vanligtvis själva ASP.NET Core program skapar ett vid startpunkten för ett program, till exempel hello `static void Main()` metod i `Program.cs`. I det här fallet är hello livscykeln för hello Webbvärden bundna toohello livscykeln för hello-processen.

![Värd för ASP.NET Core i en process][0]

Dock hello startpunkt för programmet är inte hello rätt plats toocreate en Webbvärden i en tillförlitlig tjänst, eftersom hello startpunkt för programmet används bara tooregister en tjänst-typ med hello Service Fabric runtime, så att den kan skapa instanser av tjänsten Ange. hello Webbvärden ska skapas i en tillförlitlig tjänst sig själv. Instanser av tjänsten och/eller repliker kan gå igenom flera livscykler inom värdprocess för hello-tjänsten. 

En tillförlitlig tjänstinstans representeras av din tjänstklass som härleds från `StatelessService` eller `StatefulService`. hello kommunikation stack för en tjänst som finns i en `ICommunicationListener` implementering service-klassen. Hej `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet-paket som innehåller implementeringar av `ICommunicationListener` som starta och hantera hello ASP.NET Core Webbvärden för Kestrel eller WebListener i en tillförlitlig tjänst.

![Värd för ASP.NET Core i en tillförlitlig tjänst][1]

## <a name="aspnet-core-icommunicationlisteners"></a>ASP.NET Core ICommunicationListeners
Hej `ICommunicationListener` implementeringar för Kestrel och WebListener i hello `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet-paket har liknande användningsmönster men utföra lite olika åtgärder specifika tooeach webbservern. 

Både kommunikationslyssnarna ger en konstruktor som tar hello följande argument:
 - **`ServiceContext serviceContext`**: hello `ServiceContext` objekt som innehåller information om hello tjänsten körs.
 - **`string endpointName`**: hello namnet på en `Endpoint` konfigurationen i ServiceManifest.xml. Detta är främst där hello två kommunikationslyssnarna skiljer sig: WebListener **kräver** en `Endpoint` konfiguration, men inte av Kestrel.
 - **`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: ett lambda-uttryck som du implementerar i som du skapar och returnera ett `IWebHost`. Detta gör att du tooconfigure `IWebHost` hello sätt som vanligt i ett program för ASP.NET Core. hello lambda innehåller en URL som har genererats för du beroende på hello Service Fabric-integrering alternativen du användning och hello `Endpoint` konfiguration som du anger. Att URL: en sedan kan ändras eller används som-är toostart hello-webbserver.

## <a name="service-fabric-integration-middleware"></a>Mellanprogram för Service Fabric-integrering
Hej `Microsoft.ServiceFabric.Services.AspNetCore` NuGet-paketet innehåller hello `UseServiceFabricIntegration` tilläggsmetoden på `IWebHostBuilder` som lägger till Service Fabric-medvetna mellanprogram. Den här mellanprogram konfigurerar hello Kestrel eller WebListener `ICommunicationListener` tooregister en unika tjänst-URL med hello namngivningstjänst för Service Fabric och sedan validerar klient begäranden tooensure klienter ansluter toohello tjänst. Detta är nödvändigt i en miljö med delad värden, till exempel Service Fabric där flera webbprogram kan köras på hello samma fysiska eller virtuella datorn men använder inte unika värdnamn, tooprevent klienter från att av misstag ansluta toohello fel tjänst. Det här scenariot beskrivs i detalj i hello nästa avsnitt.

### <a name="a-case-of-mistaken-identity"></a>Ett fall av felaktig identitet
Tjänsten repliker, oavsett protokoll, lyssna på en unik IP:port kombination. När en replik för tjänsten har börjat lyssna på en IP:port slutpunkt, rapporterar att slutpunkten adress toohello namngivningstjänst för Service Fabric där den kan identifieras av klienter eller andra tjänster. Om tjänster använder dynamiskt tilldelade programmet portar, en tjänst replik tillfälligtvis kan använda hello samma IP:port slutpunkten för en annan tjänst som tidigare fanns på hello samma fysiska eller virtuella datorn. Detta kan medföra att en klient toomistakely ansluta toohello fel tjänst. Detta kan inträffa om hello följande händelser inträffar:

 1. Tjänsten A lyssnar på 10.0.0.1:30000 via HTTP. 
 2. Klienten matchar A Service och hämtar adress 10.0.0.1:30000
 3. En annan nod flyttar tooa-tjänsten.
 4. Tjänsten B är placerad 10.0.0.1 och tillfälligtvis hello använder samma port 30000.
 5. Klienten försöker tooconnect tooservice A med cachelagrade adress 10.0.0.1:30000.
 6. Klienten är nu ansluten tooservice B märker inte den anslutna toohello fel tjänst.

Detta kan orsaka fel vid slumpmässiga tidpunkter som kan vara svårt toodiagnose. 

### <a name="using-unique-service-urls"></a>Med unika URL: er
tooprevent, services efter en slutpunkt toohello Naming Service med en unik identifierare och sedan validera den unika identifieraren under klientbegäranden. Detta är en samordnad åtgärd mellan tjänster i en betrodd miljö för icke-farliga-klient. Detta ger inte säkra autentiseringsuppgifter i en miljö med farliga klient.

I en betrodd miljö hello mellanprogram som läggs till av hello `UseServiceFabricIntegration` metoden lägger automatiskt till en unik identifierare toohello-adress som är bokförd toohello Naming Service och validerar den identifieraren för varje begäran. Om hello-ID inte matchar returnerar hello mellanprogram omedelbart ett HTTP 410 rest-svar.

Tjänster som använder en dynamiskt tilldelad port bör se användning av den här mellanprogram.

Tjänster som använder en fast unika port har inte det här problemet i en integrerad miljö. En fast unika port används vanligtvis för externt riktade som behöver en välkända port för klienten program tooconnect till. Till exempel använder de flesta Internet-riktade webbprogram port 80 eller 443 för web webbläsare anslutningar. I det här fallet ska hello Unik identifierare inte aktiveras.

hello följande diagram visar hello begäran flödar med hello mellanprogram aktiverad:

![Service Fabric ASP.NET Core-integrering][2]

Både Kestrel och WebListener `ICommunicationListener` implementeringar använder den här mekanismen i hello exakt samma sätt. Även om WebListener internt kan skilja förfrågningar baserat på unika URL-sökvägar med hjälp av hello underliggande *http.sys* portdelning funktion som funktionen är *inte* används av hello WebListener `ICommunicationListener` implementering eftersom som leder HTTP 503- och HTTP 404-fel statuskoder i hello-scenario som beskrivs ovan. Som gör i sin tur det mycket svårt för klienter toodetermine hello syftet med hello felet HTTP 503- och HTTP 404 är redan används ofta tooindicate andra fel. Därför både Kestrel och WebListener `ICommunicationListener` implementeringar standardisera hello mellanprogram som tillhandahålls av hello `UseServiceFabricIntegration` tilläggsmetoden så att klienter behöver bara tooperform en tjänstslutpunkt nytt lösa åtgärd på 410 HTTP-svar.

## <a name="weblistener-in-reliable-services"></a>WebListener i Reliable Services
WebListener kan användas i en tillförlitlig tjänst genom att importera hello **Microsoft.ServiceFabric.AspNetCore.WebListener** NuGet-paketet. Det här paketet innehåller `WebListenerCommunicationListener`, en implementering av `ICommunicationListener`, som gör att du toocreate en ASP.NET Core Webbvärden inuti en tillförlitlig tjänst med WebListener som hello webbserver.

WebListener bygger på hello [Windows http-Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx). Här används hello *http.sys* kernel-drivrutinen används av IIS tooprocess HTTP-begäranden och vidarebefordra dem tooprocesses webbprogram som körs. Detta gör flera processer på hello samma fysiska eller virtuella datorn toohost webbprogram på hello samma port, skiljas åt av en unik URL-sökvägen eller värdnamn. Dessa funktioner är användbara i Service Fabric som värd för flera webbplatser i hello samma kluster.

hello följande diagram illustrerar hur WebListener använder hello *http.sys* kernel-drivrutinen i Windows för delning av port:

![HTTP.sys][3]

### <a name="weblistener-in-a-stateless-service"></a>WebListener i en tillståndslös tjänst
toouse `WebListener` i en tillståndslös tjänst åsidosätta hello `CreateServiceInstanceListeners` metod och returnera ett `WebListenerCommunicationListener` instans:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseWebListener()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build()))
    };
}
```

### <a name="weblistener-in-a-stateful-service"></a>WebListener i en tillståndskänslig service

`WebListenerCommunicationListener`för närvarande är inte avsedd för användning i tillståndskänsliga tjänster på grund av toocomplications med hello underliggande *http.sys* portdelning funktionen. Mer information finns i följande avsnitt om dynamisk porttilldelning med WebListener hello. För tillståndskänsliga tjänster är Kestrel hello rekommenderas webbservern.

### <a name="endpoint-configuration"></a>Slutpunktskonfiguration

En `Endpoint` konfiguration krävs för webbservrar som använder hello API Windows HTTP-servern, inklusive WebListener. Webbservrar som använder hello Windows API för HTTP-servern måste först reservera URL-Adressen med *http.sys* (detta görs normalt med hello [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) tool). Den här åtgärden kräver förhöjd behörighet som inte har dina tjänster som standard. Hej ”http” eller ”https” alternativ för hello `Protocol` -egenskapen för hello `Endpoint` konfigurationen i *ServiceManifest.xml* används specifikt tooinstruct hello Service Fabric runtime tooregister en URL med *http.sys* för din räkning som använder hello [ *starkt jokertecken* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL-prefix.

Till exempel tooreserve `http://+:80` för en tjänst hello följande konfiguration ska användas i ServiceManifest.xml:

```xml
<ServiceManifest ... >
    ...
    <Resources>
        <Endpoints>
            <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>

</ServiceManifest>
```

Och namnet på slutpunkten hello måste överföras toohello `WebListenerCommunicationListener` konstruktorn:

```csharp
 new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     return new WebHostBuilder()
         .UseWebListener()
         .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
         .UseUrls(url)
         .Build();
 })
```

#### <a name="use-weblistener-with-a-static-port"></a>Använda WebListener med en statisk port
toouse en statisk port med WebListener, ange hello portnummer i hello `Endpoint` konfiguration:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-weblistener-with-a-dynamic-port"></a>Använda WebListener med en dynamisk port
toouse en dynamiskt tilldelad port med WebListener, utelämna hello `Port` egenskap i hello `Endpoint` konfiguration:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

Observera att en dynamisk port allokerats av en `Endpoint` konfiguration innehåller endast en port *per värdprocess*. hello aktuella Service Fabric värdmodell tillåter flera service instanser och/eller repliker toobe att finns i hello samma process, vilket innebär att var och en delar samma port vid via hello hello `Endpoint` konfiguration. Flera instanser av WebListener kan dela en port med hello underliggande *http.sys* portdelning funktionen, men som inte stöds av `WebListenerCommunicationListener` på grund av toohello komplikationer införs för klientbegäranden. För användning av dynamisk port är Kestrel hello rekommenderas webbservern.

## <a name="kestrel-in-reliable-services"></a>Kestrel i Reliable Services
Kestrel kan användas i en tillförlitlig tjänst genom att importera hello **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet-paketet. Det här paketet innehåller `KestrelCommunicationListener`, en implementering av `ICommunicationListener`, som gör att du toocreate en ASP.NET Core Webbvärden inuti en tillförlitlig tjänst med Kestrel som hello webbserver.

Kestrel är en plattformsoberoende webbserver för ASP.NET Core utifrån libuv, ett plattformsoberoende asynkrona i/o-bibliotek. Till skillnad från WebListener, Kestrel inte använder en centraliserad endpoint-hanteraren som *http.sys*. Och till skillnad från WebListener, Kestrel stöder inte portdelning mellan flera processer. Varje instans av Kestrel måste använda en unik port.

![kestrel][4]

### <a name="kestrel-in-a-stateless-service"></a>Kestrel i en tillståndslös tjänst
toouse `Kestrel` i en tillståndslös tjänst åsidosätta hello `CreateServiceInstanceListeners` metod och returnera ett `KestrelCommunicationListener` instans:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

### <a name="kestrel-in-a-stateful-service"></a>Kestrel i en tillståndskänslig service
toouse `Kestrel` i en tillståndskänslig service åsidosätta hello `CreateServiceReplicaListeners` metod och returnera ett `KestrelCommunicationListener` instans:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new ServiceReplicaListener[]
    {
        new ServiceReplicaListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                         services => services
                             .AddSingleton<StatefulServiceContext>(serviceContext)
                             .AddSingleton<IReliableStateManager>(this.StateManager))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

I det här exemplet är en singleton-instans av `IReliableStateManager` tillhandahålls toohello Webbvärden beroende injection behållare. Detta är inte absolut nödvändigt, men det gör att du toouse `IReliableStateManager` och tillförlitlig samlingar i åtgärden för MVC kontrollantmetoder.

Observera att en `Endpoint` Konfigurationsnamnet **inte** tillhandahålls för`KestrelCommunicationListener` i en tillståndskänslig service. Detta förklaras i detalj i följande avsnitt hello.

### <a name="endpoint-configuration"></a>Slutpunktskonfiguration
En `Endpoint` konfigurationen är inte obligatoriska toouse Kestrel. 

Kestrel är en enkel fristående webbservern. till skillnad från WebListener (eller HttpListener), det måste inte en `Endpoint` konfigurationen i *ServiceManifest.xml* eftersom det inte kräver tidigare toostarting för URL-registrering. 

#### <a name="use-kestrel-with-a-static-port"></a>Använda Kestrel med en statisk port
En statisk port kan konfigureras i hello `Endpoint` konfigurationen av ServiceManifest.xml för användning med Kestrel. Även om detta inte är absolut nödvändigt har två potentiella fördelar:
 1. Om hello port inte faller inom hello portintervall för programmet, öppnas den hello OS-brandväggen av Service Fabric.
 2. Hej tooyou Webbadressen som tillhandahålls via `KestrelCommunicationListener` kommer att använda den här porten.

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

Om en `Endpoint` är konfigurerat, filnamnet måste överföras i hello `KestrelCommunicationListener` konstruktorn: 

```csharp
new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) => ...
```

Om en `Endpoint` används inte konfigurering, utelämna hello namn i hello `KestrelCommunicationListener` konstruktor. I det här fallet används en dynamisk port. Se hello nedan för mer information.

#### <a name="use-kestrel-with-a-dynamic-port"></a>Använda Kestrel med en dynamisk port
Kestrel kan inte använda hello automatisk porttilldelning från hello `Endpoint` konfigurationen i ServiceManifest.xml, eftersom automatisk porttilldelning från en `Endpoint` konfigurationen tilldelar en unik port per *värd processen* , och en enda värdprocess kan innehålla flera Kestrel instanser. Eftersom Kestrel inte stöder portdelning är fungerar detta inte som varje Kestrel-instans måste öppnas på en unik port.

toouse dynamisk porttilldelning med Kestrel, bara utelämna hello `Endpoint` konfigurationen i ServiceManifest.xml helt, och inte skickar en slutpunkt namn toohello `KestrelCommunicationListener` konstruktorn:

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

I den här konfigurationen `KestrelCommunicationListener` väljer automatiskt en ledig port från hello portintervall för programmet.

## <a name="scenarios-and-configurations"></a>Scenarier och konfigurationer
Det här avsnittet beskriver hello följande scenarier och innehåller hello rekommenderas kombination av webbserver, portkonfiguration, alternativ för Service Fabric-integrering och övriga inställningar tooachieve en korrekt fungerande tjänst:
 - Externt exponeras ASP.NET Core tillståndslösa tjänsten
 - Endast internt ASP.NET Core tillståndslösa tjänsten
 - Endast internt ASP.NET Core tillståndskänslig service

En **externt exponeras** service är en som Exponerar en slutpunkt som kan nås från utanför hello kluster, vanligtvis via en belastningsutjämnare.

En **endast är interna** service är en vars endpoint kan endast nås från inom hello kluster.

> [!NOTE]
> Tillståndskänslig service slutpunkter vanligtvis inte får vara exponeras toohello Internet. Kluster som finns bakom en belastningsutjämnare som inte känner till Service Fabric-tjänsten upplösning, till exempel hello Azure belastningsutjämnare blir tooexpose tillståndskänsliga tjänster eftersom hello belastningsutjämnare inte ska kunna toolocate och vidarebefordra trafik toohello repliken är lämplig tillståndskänslig tjänst. 

### <a name="externally-exposed-aspnet-core-stateless-services"></a>Externt exponeras ASP.NET Core tillståndslösa tjänster
WebListener är hello rekommenderas webbservern för frontend-tjänster som exponerar extern, mot Internet HTTP-slutpunkter i Windows. Den ger bättre skydd mot attacker och stöder funktioner som Kestrel inte, till exempel Windows-autentisering och delning av port. 

Kestrel stöds inte som en gränsserver (Internet-inriktad) just nu. En omvänd proxy-server, till exempel IIS eller Nginx måste användas toohandle trafik från hello offentliga Internet.
 
När exponerade toohello Internet, en tillståndslösa tjänsten ska använda en välkänd och stabil slutpunkt som kan nås via en belastningsutjämnare. Detta är hello-URL som du kommer att ge toousers för programmet. Du rekommenderas hello följande konfiguration:

|  |  | **Anteckningar** |
| --- | --- | --- |
| Webbserver | WebListener | Om hello-tjänsten är bara synliga tooa betrott nätverk, ett intranät, kan Kestrel användas. Annars är WebListener hello önskade alternativ. |
| Portkonfiguration | Statisk | En välkänd statisk port ska konfigureras i hello `Endpoints` konfigurationen av ServiceManifest.xml, till exempel 80 för HTTP och 443 för HTTPS. |
| ServiceFabricIntegrationOptions | Ingen | Hej `ServiceFabricIntegrationOptions.None` bör användas när du konfigurerar Service Fabric-integrering mellanprogram så att hello-tjänsten inte försöker toovalidate inkommande begäranden för en unik identifierare. Externa användare av ditt program vet inte hello unika identifieringsinformation används av hello mellanprogram. |
| Antal instanser | -1 | I vanliga användningsområden hello instansantalet inställningen anges för ”-1” så att en instans är tillgänglig på alla noder som tar emot trafik från en belastningsutjämnare. |

Om flera tjänster som externt synliga delar hello samma uppsättning noder, ska en unik men stabil URL-sökvägen användas. Detta kan åstadkommas genom att ändra hello URL: en när du konfigurerar IWebHost. Observera att detta gäller endast tooWebListener.

 ```csharp
 new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     url += "/MyUniqueServicePath";
 
     return new WebHostBuilder()
         .UseWebListener()
         ...
         .UseUrls(url)
         .Build();
 })
 ```

### <a name="internal-only-stateless-aspnet-core-service"></a>Endast internt tillståndslösa ASP.NET Core-tjänsten
Tillståndslösa tjänster som endast anropas från inom hello klustret ska använda unika URL: er och dynamiskt tilldelade portar tooensure samarbete mellan flera tjänster. Du rekommenderas hello följande konfiguration:

|  |  | **Anteckningar** |
| --- | --- | --- |
| Webbserver | kestrel | Även om WebListener kan användas för interna tillståndslösa tjänster, är Kestrel hello rekommenderade server tooallow flera service instanser tooshare en värd.  |
| Portkonfiguration | dynamiskt tilldelade | Flera repliker på en tillståndskänslig tjänst kan dela en värdprocess eller värdoperativsystemet och därför måste unika portar. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Den här inställningen förhindrar med dynamisk porttilldelning hello blivit identitet problemet som beskrivs ovan. |
| InstanceCount | alla | Hej instansantalet inställningen kan ställas in tooany värdet nödvändiga toooperate hello-tjänsten. |

### <a name="internal-only-stateful-aspnet-core-service"></a>Endast internt tillståndskänslig ASP.NET Core-tjänsten
Tillståndskänsliga tjänster som endast anropas från inom hello klustret ska använda dynamiskt tilldelade portar tooensure samarbete mellan flera tjänster. Du rekommenderas hello följande konfiguration:

|  |  | **Anteckningar** |
| --- | --- | --- |
| Webbserver | kestrel | Hej `WebListenerCommunicationListener` är inte avsedd för användning av tillståndskänsliga tjänster där repliker delar en värdprocess. |
| Portkonfiguration | dynamiskt tilldelade | Flera repliker på en tillståndskänslig tjänst kan dela en värdprocess eller värdoperativsystemet och därför måste unika portar. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Den här inställningen förhindrar med dynamisk porttilldelning hello blivit identitet problemet som beskrivs ovan. |

## <a name="next-steps"></a>Nästa steg
[Felsöka Service Fabric-program med hjälp av Visual Studio](service-fabric-debugging-your-application.md)

<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
