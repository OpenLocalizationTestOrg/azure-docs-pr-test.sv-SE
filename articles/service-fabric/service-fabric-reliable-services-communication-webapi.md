---
title: "Kommunikation med ASP.NET Web API-tjänsten | Microsoft Docs"
description: "Lär dig hur du implementerar tjänstkommunikation steg för steg genom att använda ASP.NET Web API med OWIN värd själv i Reliable Services API."
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
ms.openlocfilehash: 73b7e1c0cb93ae7c54780a3aab837b0e5bcdb0a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Komma igång: Service Fabric Web API-tjänster med OWIN själva värd
Azure Service Fabric placerar kraften i dina händer när du bestämmer hur du vill att dina tjänster att kommunicera med användare och med varandra. Den här självstudiekursen fokuserar på implementering av kommunikation med Open Web Interface för .NET (OWIN) värd själv i Service Fabric Reliable Services API ASP.NET Web API. Vi kommer detaljerad information om djupt pluggable kommunikation Reliable Services API. Vi använder också Web API i ett steg för steg-exempel som visar hur du ställer in en anpassad kommunikation lyssnare.

## <a name="introduction-to-web-api-in-service-fabric"></a>Introduktion till webb-API i Service Fabric
ASP.NET Web API är ett populärt och kraftfullt ramverk för att skapa HTTP APIs ovanpå .NET Framework. Om du inte redan är bekant med framework, se [komma igång med ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) vill veta mer.

Webb-API i Service Fabric är samma ASP.NET Web API du känner till och. Skillnaden är i hur du *värden* ett Web API-program. Du kommer inte att använda Microsoft Internet Information Services (IIS). För att bättre förstå skillnaden kan vi dela i två delar:

1. Web API-program (inklusive domänkontrollanter och modeller)
2. Värden (webbservern, vanligtvis IIS)

Ett Web API-program ändras inte. Det är inte detsamma Web API-program som du har skrivit tidigare och du bör kunna helt enkelt flytta över de flesta av programkoden. Men om du värd för IIS, där värden programmet kanske skiljer sig något från vad du ofta. Innan det är dags att värd, låt oss börja med något mer bekant: Web API-program.

## <a name="create-the-application"></a>Skapa programmet
Börja med att skapa ett nytt Service Fabric-program med en enda tillståndslös tjänst i Visual Studio 2015.

Visual Studio-mall för en tillståndslös tjänst med hjälp av Web API är tillgängliga för dig. I den här kursen ska vi skapa ett Web API-projekt från grunden som resulterar i vad du skulle få om du har valt den här mallen.

Välj ett tomt projekt tillståndslösa tjänsten att lära dig hur du skapar ett Web API-projekt från grunden eller du kan starta med hjälp av tillståndslösa tjänsten Web API-mallen och bara följa med.  

Det första steget är att dra in vissa NuGet-paket för Web API. Vi vill använda paketet är Microsoft.AspNet.WebApi.OwinSelfHost. Det här paketet innehåller alla nödvändiga Web API-paket och *värden* paket. Det här är viktigt senare.

När paketen har installerats kan börja du skapa grundstrukturen för Web API-projekt. Om du har använt Web API projektstrukturen bör mycket Se bekant ut. Starta genom att lägga till en `Controllers` katalog och en domänkontrollant med enkla värden:

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

Sedan lägger till en startklass i projektroten att registrera routning, formaterare och andra konfigurationsinställningar. Det är också där Web API ansluts till den *värden*, som ska granskas igen senare. 

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

Det stämmer för programmet del. Nu har vi skapat bara grundläggande Web API-Projektlayout. Hittills har det får inte mycket se annorlunda ut från Web API-projekt som du har skrivit tidigare eller från den grundläggande Web API-mallen. Affärslogik försätts i domänkontrollanter och modeller som vanligt.

Nu vad vi gör om värd så att vi kan faktiskt kör den?

## <a name="service-hosting"></a>Tjänstevärden
I Service Fabric-tjänsten körs i en *tjänsten värdprocess*, en körbar fil som körs koden för tjänsten. När du skriver en tjänst med hjälp av tillförlitliga Services API kompilerar bara service-projekt till en körbar fil som registrerar service-typen och kör din kod. Detta gäller i de flesta fall när du skriver en tjänst på Service Fabric i .NET. När du öppnar Program.cs i projektet tillståndslösa tjänsten bör du se:

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

Om du som ser misstänkt ut startpunkten för ett konsolprogram, som är eftersom den är.

Mer information om tjänsten värdprocess och tjänstregistrering ligger utanför omfånget för den här artikeln. Men det är viktigt att veta för nu *service koden körs i sin egen process*.

## <a name="self-host-web-api-with-an-owin-host"></a>Automatisk värd för webb-API med en OWIN-värd
Med tanke på att programkoden webb-API finns i en egen process, hur du koppla den till en webbserver? Ange [OWIN](http://owin.org/). OWIN är bara ett avtal mellan .NET-webbprogram och webbservrar. Traditionellt när ASP.NET (upp till MVC 5) används kopplas webbprogrammet nära till IIS via System.Web. Men implementerar Web API OWIN, så du kan skriva ett program som är fristående från webbservern som värd för den. Därför kan du använda en *egenvärdbaserat* OWIN-webbserver som du kan starta i en egen process. Detta passar perfekt med Service Fabric-värdmodell vi bara beskrivs.

I den här artikeln använder vi Katana som värd för OWIN för Web API-program. Katana är en öppen källkod OWIN värd-implementering som bygger på [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) och Windows [http-Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

> [!NOTE]
> Mer information om Katana går du till den [Katana plats](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). En snabb överblick över hur du använder Katana själva värd för webb-API finns [använder OWIN till Self-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).
> 
> 

## <a name="set-up-the-web-server"></a>Konfigurera webbservern
Reliable Services API ger en startpunkt för kommunikation där du kan ansluta i kommunikation staplar som gör att användare och klienter kan ansluta till tjänsten:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

Webbservern (och eventuella andra kommunikation stack du använder i framtiden, till exempel WebSockets) ska använda gränssnittet ICommunicationListener för att integrera korrekt med systemet. Orsaker till detta kommer att bli tydligare i följande steg.

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

Gränssnittet ICommunicationListener omfattar tre metoder för att hantera en lyssnare för kommunikation för din tjänst:

* *OpenAsync*. Börja lyssna efter begäranden.
* *CloseAsync*. Slutar att lyssna efter begäranden, Slutför alla pågående begäranden och avslutas på vanligt sätt.
* *Avbryt*. Avbryt allt och stoppa omedelbart.

Kom igång genom att lägga till privata klassmedlemmar för lyssnaren behöver för att fungera. Dessa initieras via konstruktorn och användas senare när du ställer in lyssnande URL: en.

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
Om du vill konfigurera webbservern, behöver du två typer av information:

* *Ett URL-prefix för sökvägen*. Det här är valfritt, är men bra att konfigurera detta nu så att du kan på ett säkert sätt vara värd för flera webbtjänster i ditt program.
* *En port*.

Det är viktigt att du förstår att Service Fabric ger ett lager för program som fungerar som en buffert mellan programmet och det underliggande operativsystemet som körs på innan du får en port på webbservern. Därför Service Fabric är ett sätt att konfigurera *slutpunkter* för dina tjänster. Service Fabric säkerställer att slutpunkter är tillgängliga att använda tjänsten. På så sätt kan du inte behöver konfigurera dem själv i den underliggande OS-miljön. Du kan enkelt värd Service Fabric programmet i olika miljöer utan att behöva göra ändringar i ditt program. (Du kan till exempel värd samma program i Azure eller i ditt eget datacenter.)

Konfigurera en HTTP-slutpunkt i PackageRoot\ServiceManifest.xml:

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Det här steget är viktigt eftersom värdprocessen för service körs under autentiseringsuppgifterna för begränsade (Network Service i Windows). Det innebär att tjänsten inte har åtkomst till ställa in en HTTP-slutpunkt på sin egen. Med hjälp av slutpunktskonfigurationen vet Service Fabric för att ställa in rätt åtkomstkontrollistan (ACL) för den URL som tjänsten lyssnar på. Service Fabric innehåller också en standardplats för att konfigurera slutpunkter.

Tillbaka i OwinCommunicationListener.cs börjar du implementera OpenAsync. Det är där du startar webbservern. Först hämta slutpunktsinformation om och skapa den URL som tjänsten lyssnar på. URL: en är olika beroende på om lyssnaren används i en tillståndslös tjänst eller en tillståndskänslig service. För en tillståndskänslig service måste lyssnaren skapa en unik adress för varje tillståndskänslig service replik som den lyssnar på. Adressen kan vara mycket enklare för tillståndslösa tjänster. 

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

Observera att ”http://+” användas här. Detta är att se till att servern lyssnar på alla tillgängliga adresser, inklusive localhost, FQDN och IP-Adressen för datorn.

OpenAsync-implementering är ett av de viktigaste skälen varför webbservern (eller någon kommunikation stack) implementeras som en ICommunicationListener, istället för bara den öppna direkt från `RunAsync()` i tjänsten. Returvärdet från OpenAsync är den adress som lyssnar på webbservern. När den här adressen returneras till systemet, registrerar adressen med tjänsten. Service Fabric tillhandahåller ett API som gör att klienter och andra tjänster kan sedan ber för den här adressen av tjänstnamn. Detta är viktigt eftersom serviceadressen inte är statisk. Tjänster flyttas i klustret för resursen belastningsutjämning och tillgänglighet. Det här är den mekanism som gör att klienter kan matcha lyssnande adressen för en tjänst.

Med detta i åtanke OpenAsync startar webbservern och returnerar adressen som lyssnar på. Observera att den lyssnar på ”http://+”, men innan OpenAsync Returnerar adressen på ”+” ersätts med IP- eller FQDN för den används för närvarande på noden. Adressen som returneras av den här metoden är vad som har registrerats med systemet. Det är också klienter och andra tjänster ser när de frågar efter en tjänst-adress. För klienter att ansluta korrekt till den, behöver de en faktiska IP eller FQDN i adressen.

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
        this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

Observera att detta refererar till Start-klassen som skickades till OwinCommunicationListener i konstruktorn. Den här Start-instansen används av webbservern för att starta Web API-program.

Den `ServiceEventSource.Current.Message()` rad visas i fönstret diagnostiska händelser senare, när du kör programmet för att bekräfta att servern har startats.

## <a name="implement-closeasync-and-abort"></a>Implementera CloseAsync och ITransaction::Abort
Slutligen implementera både CloseAsync och Avbryt om du vill stoppa webbservern. Webbservern kan stoppas av avyttring server-referensen som skapades under OpenAsync.

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

I det här implementeringsexemplet stoppa både CloseAsync och Avbryt bara webbservern. Du kan välja för att utföra en mer korrekt samordnade avstängning för webbservern i CloseAsync. Avstängningen kan vänta pågående begäranden ska slutföras innan tillbaka.

## <a name="start-the-web-server"></a>Starta webbservern
Du är nu redo att skapa och returnera en instans av OwinCommunicationListener att starta webbservern. Tillbaka i klassen Service (WebService.cs) åsidosätta den `CreateServiceInstanceListeners()` metoden:

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

Det är där webb-API *programmet* och OWIN *värden* slutligen uppfyller. Värden (OwinCommunicationListener) ges en instans av den *programmet* (webb-API) via Start-klassen. Service Fabric hanterar sedan dess livscykel. Den här samma mönster kan vanligtvis följas med någon kommunikation stack.

## <a name="put-it-all-together"></a>Färdigställa allt
I det här exemplet behöver du inte göra något den `RunAsync()` metod, så att åsidosättningen kan bara tas bort.

Den slutliga implementeringen ska vara mycket enkelt. Den behöver bara skapa lyssnaren kommunikation:

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

Det fullständiga `OwinCommunicationListener` klass:

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
                this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

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

Nu när du har placerat alla delar på plats, ditt projekt bör se ut som ett typiskt Web API-program med tillförlitlig Services API startpunkter och en OWIN-värd.

## <a name="run-and-connect-through-a-web-browser"></a>Kör och ansluter via en webbläsare
Om du inte gjort det, [ställa in din utvecklingsmiljö](service-fabric-get-started.md).

Du kan nu skapa och distribuera din tjänst. Tryck på **F5** i Visual Studio för att skapa och distribuera programmet. I fönstret diagnostiska händelser bör du se ett meddelande som anger att webbservern öppnas på http://localhost:8281 /.

> [!NOTE]
> Om porten har redan öppnats av någon annan process på datorn, visas ett felmeddelande. Detta anger att lyssnaren inte kunde öppnas. Om så är fallet kan du prova att använda en annan port för slutpunktskonfigurationen i ServiceManifest.xml.
> 
> 

När tjänsten körs kan öppna en webbläsare och gå till [http://localhost:8281-api-värden](http://localhost:8281/api/values) att testa den.

## <a name="scale-it-out"></a>Skala ut
Skala ut tillståndslös webbprogram vanligtvis innebär att lägga till flera datorer och igång webbprogram på dem.. Service Fabric orchestration-motorn kan göra när nya noder läggs till ett kluster. När du skapar instanser av tillståndslösa tjänsten måste ange du antalet instanser som du vill skapa. Service Fabric placeras det antalet instanser på noder i klustret. Och det gör att inte skapa fler än en instans på varje nod i ett. Du kan också instruera Service Fabric du alltid vill skapa en instans på varje nod genom att ange **-1** för instansantal. Detta garanterar att när du lägger till noder för att skala ut ditt kluster en instans av tillståndslösa tjänsten kommer att skapas på de nya noderna. Det här värdet är en egenskap för tjänstinstansen, så anges när du skapar en instans av tjänsten. Du kan göra detta med hjälp av PowerShell:

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

Mer information om hur du skapar program och instanser av tjänsten finns [distribuera ett program](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Nästa steg
[Felsöka Service Fabric-program med hjälp av Visual Studio](service-fabric-debugging-your-application.md)

