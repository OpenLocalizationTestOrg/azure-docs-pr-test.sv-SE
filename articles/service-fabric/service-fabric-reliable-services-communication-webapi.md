---
title: aaaService kommunikation med hello ASP.NET Web API | Microsoft Docs
description: "Lär dig hur tooimplement kommunikation steg för steg genom att använda hello ASP.NET Web API med OWIN värd själv i hello Reliable Services API."
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
ms.date: 02/10/2017
ms.author: vturecek
redirect_url: /azure/service-fabric/service-fabric-reliable-services-communication-aspnetcore
ms.openlocfilehash: 3fb18fcb141ada0d79a0acda3dccbc7fb044346d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Komma igång: Service Fabric Web API-tjänster med OWIN själva värd
Azure Service Fabric placerar hello power i händerna när du bestämmer du hur dina tjänster toocommunicate med användare och med varandra. Den här självstudiekursen fokuserar på implementering av kommunikation med Open Web Interface för .NET (OWIN) värd själv i Service Fabric Reliable Services API ASP.NET Web API. Vi kommer detaljerad information om djupt hello Reliable Services pluggable kommunikation API. Vi använder också Web API i en detaljerade tooshow du hur tooset upp en anpassad kommunikation lyssnare.

## <a name="introduction-tooweb-api-in-service-fabric"></a>Introduktion tooWeb API i Service Fabric
ASP.NET Web API är ett populärt och kraftfullt ramverk för att skapa HTTP APIs ovanpå hello .NET Framework. Om du inte redan är bekant med hello framework, se [komma igång med ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) toolearn mer.

Webb-API i Service Fabric är hello samma ASP.NET Web API du känner till och. hello skillnaden är i hur du *värden* ett Web API-program. Du kommer inte att använda Microsoft Internet Information Services (IIS). toobetter förstå hello skillnaden, dela vi i två delar:

1. hello Web API-program (inklusive domänkontrollanter och modeller)
2. hello-värden (hello webbserver, vanligtvis IIS)

Ett Web API-program ändras inte. Det är inte detsamma Web API-program som du har skrivit hello senaste och bör du kunna toosimply flytta över de flesta av programkoden. Men om du har värd i IIS, där du vara värd för programmet hello kanske skiljer sig något från vad du är van vid. Innan vi hämta toohello som värd för en del vi börjar med något mer bekant: hello Web API-program.

## <a name="create-hello-application"></a>Skapa hello program
Börja med att skapa ett nytt Service Fabric-program med en enda tillståndslös tjänst i Visual Studio 2015.

Visual Studio-mall för en tillståndslös tjänst med hjälp av Web API är tillgängliga tooyou. I den här kursen ska vi skapa ett Web API-projekt från grunden som resulterar i vad du skulle få om du har valt den här mallen.

Välj en tom tillståndslösa tjänsten projektet toolearn hur toobuild Web API-projekt från grunden eller du kan börja med hello tillståndslösa tjänsten Web API-mallen och bara följa med.  

hello första steget är toopull i vissa NuGet-paket för Web API. hello-paket som vi vill toouse är Microsoft.AspNet.WebApi.OwinSelfHost. Det här paketet innehåller alla hello nödvändiga Web API-paket och hello *värden* paket. Det här är viktigt senare.

När du har installerat hello-paket, kan du börja bygga ut hello Web API-projekt grundstrukturen. Om du har använt Web API hello projektstruktur bör mycket Se bekant ut. Starta genom att lägga till en `Controllers` katalog och en domänkontrollant med enkla värden:

**ValuesController.cs**

```csharp
using System.Collections.Generic;
using System.Web.Http;

namespace WebService.Controllers
{
    public class ValuesController : ApiController
    {
        // GET api/values 
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5 
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values 
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5 
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5 
        public void Delete(int id)
        {
        }
    }
}

```

Sedan lägger till en startklass på hello projektets rot tooregister hello routning, formaterare och andra konfigurationsinställningar. Det är också där Web API ansluts toohello *värden*, som ska granskas igen senare. 

**Startup.CS**

```csharp
using System.Web.Http;
using Owin;

namespace WebService
{
    public static class Startup
    {
        public static void ConfigureApp(IAppBuilder appBuilder)
        {
            // Configure Web API for self-host. 
            HttpConfiguration config = new HttpConfiguration();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            appBuilder.UseWebApi(config);
        }
    }
}
```

Det stämmer för programmet hello. Nu har vi skapat bara hello grundläggande Web API-projekt layouten. Hittills har det bör inte mycket se annorlunda ut från Web API-projekt som du har skrivit hello senaste eller hello grundläggande Web API-mall. Affärslogik försätts i hello domänkontrollanter och modeller som vanligt.

Nu vad vi gör om värd så att vi kan faktiskt kör den?

## <a name="service-hosting"></a>Tjänstevärden
I Service Fabric-tjänsten körs i en *tjänsten värdprocess*, en körbar fil som körs koden för tjänsten. När du skriver en tjänst med hjälp av hello Reliable Services API kompilerar tjänstens projekt bara tooan körbar fil som registrerar service-typen och kör din kod. Detta gäller i de flesta fall när du skriver en tjänst på Service Fabric i .NET. När du öppnar Program.cs i projektet för hello tillståndslösa tjänsten bör du se:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric.Services.Runtime;

internal static class Program
{
    private static void Main()
    {
        try
        {
            ServiceRuntime.RegisterServiceAsync("WebServiceType",
                context => new WebService(context)).GetAwaiter().GetResult();

            ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(WebService).Name);

            // Prevents this host process from terminating so services keeps running. 
            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Om du som ser misstänkt ut hello post punkt tooa konsolprogram som är eftersom den är.

Mer information om hello serverprocess och tjänstregistrering ligger utanför hello omfånget för den här artikeln. Men det är viktigt tooknow för nu *service koden körs i sin egen process*.

## <a name="self-host-web-api-with-an-owin-host"></a>Automatisk värd för webb-API med en OWIN-värd
Med tanke på att programkoden webb-API finns i en egen process, hur du koppla den tooa webbserver? Ange [OWIN](http://owin.org/). OWIN är bara ett avtal mellan .NET-webbprogram och webbservrar. När ASP.NET (upp tooMVC 5) används är traditionellt tätt kopplade tooIIS via System.Web hello webbprogram. Men implementerar Web API OWIN, så du kan skriva ett program som är fristående från hello-webbserver som är värd för den. Därför kan du använda en *egenvärdbaserat* OWIN-webbserver som du kan starta i en egen process. Detta passar perfekt med hello Service Fabric värdmodell vi bara beskrivs.

I den här artikeln använder vi Katana som hello OWIN-värd för hello Web API-program. Katana är en öppen källkod OWIN värd-implementering som bygger på [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) och hello Windows [http-Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

> [!NOTE]
> Mer information om Katana, gå toohello toolearn [Katana plats](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). För en snabb överblick över hur toouse Katana tooself värden Web API finns [Använd OWIN tooSelf värden ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).
> 
> 

## <a name="set-up-hello-web-server"></a>Konfigurera hello webbserver
hello Reliable Services API ger en startpunkt för kommunikation där du kan ansluta i kommunikation staplar som gör att användare och klienter tooconnect toohello tjänsten:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

hello webbserver (och eventuella andra kommunikation stack du använder i hello framtida, till exempel WebSockets) ska använda hello ICommunicationListener gränssnittet toointegrate korrekt med hello system. hello orsaker till detta kommer att bli tydligare i hello följande steg.

Först skapar du en klass med namnet OwinCommunicationListener som implementerar ICommunicationListener:

**OwinCommunicationListener.cs**

```csharp
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;
using System;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}
```

Hej ICommunicationListener gränssnittet innehåller tre metoder toomanage en lyssnare för kommunikation för din tjänst:

* *OpenAsync*. Börja lyssna efter begäranden.
* *CloseAsync*. Slutar att lyssna efter begäranden, Slutför alla pågående begäranden och avslutas på vanligt sätt.
* *Avbryt*. Avbryt allt och stoppa omedelbart.

tooget igång, lägga till privata klassmedlemmar för saker hello lyssnare måste toofunction. Dessa initieras via hello konstruktorn och användas senare när du ställer in hello lyssnar URL.

```csharp
internal class OwinCommunicationListener : ICommunicationListener
{
    private readonly ServiceEventSource eventSource;
    private readonly Action<IAppBuilder> startup;
    private readonly ServiceContext serviceContext;
    private readonly string endpointName;
    private readonly string appRoot;

    private IDisposable webApp;
    private string publishAddress;
    private string listeningAddress;

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
        : this(startup, serviceContext, eventSource, endpointName, null)
    {
    }

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
    {
        if (startup == null)
        {
            throw new ArgumentNullException(nameof(startup));
        }

        if (serviceContext == null)
        {
            throw new ArgumentNullException(nameof(serviceContext));
        }

        if (endpointName == null)
        {
            throw new ArgumentNullException(nameof(endpointName));
        }

        if (eventSource == null)
        {
            throw new ArgumentNullException(nameof(eventSource));
        }

        this.startup = startup;
        this.serviceContext = serviceContext;
        this.endpointName = endpointName;
        this.eventSource = eventSource;
        this.appRoot = appRoot;
    }


    ...

```

## <a name="implement-openasync"></a>Implementera OpenAsync
tooset in hello webbserver, behöver du två typer av information:

* *Ett URL-prefix för sökvägen*. Även om det här är valfritt, är det bra om du tooset detta nu upp så att du kan på ett säkert sätt vara värd för flera webbtjänster i ditt program.
* *En port*.

Det är viktigt att du förstår att Service Fabric ger ett lager för program som fungerar som en buffert mellan programmet och hello underliggande operativsystemet som körs på innan du får en port för hello webbservern. Service Fabric tillhandahåller en sätt tooconfigure *slutpunkter* för dina tjänster. Service Fabric säkerställer att slutpunkter är tillgängliga för din tjänst toouse. På så sätt kan du inte har tooconfigure dem själv i hello underliggande OS-miljö. Du kan enkelt värd Service Fabric programmet i olika miljöer utan att behöva toomake alla ändringar tooyour program. (Du kan till exempel värd hello samma program i Azure eller i ditt eget datacenter.)

Konfigurera en HTTP-slutpunkt i PackageRoot\ServiceManifest.xml:

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Det här steget är viktigt eftersom hello värdprocessen för service körs under autentiseringsuppgifterna för begränsade (Network Service i Windows). Det innebär att tjänsten inte har åtkomst tooset in en HTTP-slutpunkt på sin egen. Med hjälp av hello slutpunktskonfiguration vet Service Fabric tooset in hello rätt åtkomstkontrollista (ACL) för hello URL som hello tjänsten lyssnar på. Service Fabric innehåller också en standardplats tooconfigure slutpunkter.

Tillbaka i OwinCommunicationListener.cs börjar du implementera OpenAsync. Det är där du startar hello webbservern. Först hämta hello slutpunktsinformation och skapa hello-URL som hello tjänsten lyssnar på. hello URL är olika beroende på om hello lyssnare används i en tillståndslös tjänst eller en tillståndskänslig service. För en tillståndskänslig service måste hello lyssnare toocreate en unik adress för varje replik tillståndskänslig service den lyssnar på. Hello-adressen kan vara mycket enklare för tillståndslösa tjänster. 

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
    var protocol = serviceEndpoint.Protocol;
    int port = serviceEndpoint.Port;

    if (this.serviceContext is StatefulServiceContext)
    {
        StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}{3}/{4}/{5}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/',
            statefulServiceContext.PartitionId,
            statefulServiceContext.ReplicaId,
            Guid.NewGuid());
    }
    else if (this.serviceContext is StatelessServiceContext)
    {
        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/');
    }
    else
    {
        throw new InvalidOperationException();
    }

    ...

```

Observera att ”http://+” användas här. Detta är toomake till webbservern hello lyssnar på alla tillgängliga adresser, inklusive localhost, FQDN och hello dator-IP.

hello OpenAsync implementering är ett av hello viktigaste skälen varför hello webbserver (eller någon kommunikation stack) implementeras som en ICommunicationListener, istället för bara den öppna direkt från `RunAsync()` i hello-tjänsten. hello returvärde från OpenAsync är hello-adress som hello webbservern lyssnar på. När den här adressen returneras toohello system, registrerar hello-adress med hello-tjänsten. Service Fabric tillhandahåller ett API som gör att klienter och andra tjänster toothen fråga för den här adressen av tjänstnamn. Detta är viktigt eftersom hello-adress inte är statisk. Tjänster flyttas i hello kluster för resursen belastningsutjämning och tillgänglighet. Detta är hello mekanism som gör att klienter tooresolve hello lyssnar-adress för en tjänst.

Med detta i åtanke OpenAsync startar hello webbservern och returnerar hello-adressen som lyssnar på. Observera att den lyssnar på ”http://+”, men innan OpenAsync returnerar hello adress hello ”+” ersätts med hello IP eller FQDN för hello nod den för närvarande finns i. hello-adressen som returneras av den här metoden är vad som har registrerats med hello system. Det är också klienter och andra tjänster ser när de frågar efter en tjänst-adress. För klienter toocorrectly ansluta tooit, de behöver en faktiska IP eller FQDN i hello-adress.

```csharp
    ...

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    try
    {
        this.eventSource.Message("Starting web server on " + this.listeningAddress);

        this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

        this.eventSource.Message("Listening on " + this.publishAddress);

        return Task.FromResult(this.publishAddress);
    }
    catch (Exception ex)
    {
        this.eventSource.Message("Web server failed tooopen endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

Observera att detta refererar hello-startklass som skickades i toohello OwinCommunicationListener i hello konstruktor. Den här Start-instansen används av hello web server toobootstrap hello Web API-program.

Hej `ServiceEventSource.Current.Message()` raden visas i fönstret för hello diagnostiska händelser senare, när du kör hello programmet tooconfirm hello webbservern har startats.

## <a name="implement-closeasync-and-abort"></a>Implementera CloseAsync och ITransaction::Abort
Slutligen implementerar både CloseAsync och Avbryt toostop hello-webbserver. hello webbservern kan stoppas av avyttring hello server hantera som skapades under OpenAsync.

```csharp
public Task CloseAsync(CancellationToken cancellationToken)
{
    this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.webApp != null)
    {
        try
        {
            this.webApp.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}
```

I det här implementeringsexemplet stoppa både CloseAsync och Avbryt bara hello webbserver. Du kan välja tooperform mer smidigt samordnade stängas av hello webbserver i CloseAsync. Hello avstängning kan vänta pågående begäranden toobe slutförts innan hello returnerar.

## <a name="start-hello-web-server"></a>Starta hello webbserver
Du är nu redo toocreate och returnera en instans av OwinCommunicationListener toostart hello-webbserver. Tillbaka i hello tjänstklass (WebService.cs), åsidosätta hello `CreateServiceInstanceListeners()` metoden:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                           .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                           .Select(endpoint => endpoint.Name);

    return endpoints.Select(endpoint => new ServiceInstanceListener(
        serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
}
```

Det är där hello Web API *programmet* och hello OWIN *värden* slutligen uppfyller. hello-värden (OwinCommunicationListener) ges en instans av hello *programmet* (webb-API) via hello-startklass. Service Fabric hanterar sedan dess livscykel. Den här samma mönster kan vanligtvis följas med någon kommunikation stack.

## <a name="put-it-all-together"></a>Färdigställa allt
I det här exemplet behöver du inte toodo allt annat i hello `RunAsync()` metod, så att åsidosättningen kan bara tas bort.

hello slutliga implementering ska vara mycket enkelt. Det bara behöver toocreate hello kommunikation lyssnare:

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Linq;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace WebService
{
    internal sealed class WebService : StatelessService
    {
        public WebService(StatelessServiceContext context)
            : base(context)
        { }

        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                                   .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                                   .Select(endpoint => endpoint.Name);

            return endpoints.Select(endpoint => new ServiceInstanceListener(
                serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
        }
    }
}
```

hello fullständig `OwinCommunicationListener` klass:

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        private readonly ServiceEventSource eventSource;
        private readonly Action<IAppBuilder> startup;
        private readonly ServiceContext serviceContext;
        private readonly string endpointName;
        private readonly string appRoot;

        private IDisposable webApp;
        private string publishAddress;
        private string listeningAddress;

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
            : this(startup, serviceContext, eventSource, endpointName, null)
        {
        }

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
        {
            if (startup == null)
            {
                throw new ArgumentNullException(nameof(startup));
            }

            if (serviceContext == null)
            {
                throw new ArgumentNullException(nameof(serviceContext));
            }

            if (endpointName == null)
            {
                throw new ArgumentNullException(nameof(endpointName));
            }

            if (eventSource == null)
            {
                throw new ArgumentNullException(nameof(eventSource));
            }

            this.startup = startup;
            this.serviceContext = serviceContext;
            this.endpointName = endpointName;
            this.eventSource = eventSource;
            this.appRoot = appRoot;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
            var protocol = serviceEndpoint.Protocol;
            int port = serviceEndpoint.Port;

            if (this.serviceContext is StatefulServiceContext)
            {
                StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}{3}/{4}/{5}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/',
                    statefulServiceContext.PartitionId,
                    statefulServiceContext.ReplicaId,
                    Guid.NewGuid());
            }
            else if (this.serviceContext is StatelessServiceContext)
            {
                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/');
            }
            else
            {
                throw new InvalidOperationException();
            }

            this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            try
            {
                this.eventSource.Message("Starting web server on " + this.listeningAddress);

                this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

                this.eventSource.Message("Listening on " + this.publishAddress);

                return Task.FromResult(this.publishAddress);
            }
            catch (Exception ex)
            {
                this.eventSource.Message("Web server failed tooopen endpoint {0}. {1}", this.endpointName, ex.ToString());

                this.StopWebServer();

                throw;
            }
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.webApp != null)
            {
                try
                {
                    this.webApp.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}
```

Nu när du har placerat alla hello delar på plats, ditt projekt bör se ut som ett typiskt Web API-program med tillförlitlig Services API startpunkter och en OWIN-värd.

## <a name="run-and-connect-through-a-web-browser"></a>Kör och ansluter via en webbläsare
Om du inte gjort det, [ställa in din utvecklingsmiljö](service-fabric-get-started.md).

Du kan nu skapa och distribuera din tjänst. Tryck på **F5** i Visual Studio toobuild och distribuera hello program. I hello diagnostiska händelser fönster bör du se ett meddelande som anger hello webbservern öppnas på http://localhost:8281 /.

> [!NOTE]
> Om hello port har redan öppnats av någon annan process på datorn, visas ett felmeddelande. Detta anger att hello-lyssnaren inte kunde öppnas. Om så är fallet hello kan du prova att använda en annan port för hello slutpunktskonfiguration i ServiceManifest.xml.
> 
> 

När hello-tjänsten körs, öppna en webbläsare och gå för[http://localhost:8281-api-värden](http://localhost:8281/api/values) tootest den.

## <a name="scale-it-out"></a>Skala ut
Skala ut tillståndslös webbprogram vanligtvis innebär att lägga till flera datorer och igång hello webbprogram på dem.. Service Fabric orchestration-motorn kan göra när nya noder läggs tooa klustret. När du skapar instanser av tillståndslösa tjänsten måste du ange hello många instanser som du vill toocreate. Service Fabric placeras det antalet instanser på noderna i klustret hello. Och det gör att toocreate inte fler än en instans på varje nod i ett. Du kan också instruera Service Fabric tooalways skapa en instans på varje nod genom att ange **-1** för hello instanser. Detta garanterar att när du lägger till tooscale noder i klustret en instans av tillståndslösa tjänsten kommer att skapas på hello nya noder. Det här värdet är en egenskap för hello tjänstinstans, så anges när du skapar en instans av tjänsten. Du kan göra detta med hjälp av PowerShell:

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

Du kan också göra detta när du definierar en standardtjänst i ett Visual Studio-projekt för tillståndslösa tjänsten:

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

Mer information om hur toocreate program och instanser av tjänsten, se [distribuera ett program](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Nästa steg
[Felsöka Service Fabric-program med hjälp av Visual Studio](service-fabric-debugging-your-application.md)

