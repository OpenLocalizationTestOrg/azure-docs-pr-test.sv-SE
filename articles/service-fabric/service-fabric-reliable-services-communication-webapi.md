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
# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a><span data-ttu-id="39ccf-103">Komma igång: Service Fabric Web API-tjänster med OWIN själva värd</span><span class="sxs-lookup"><span data-stu-id="39ccf-103">Get started: Service Fabric Web API services with OWIN self-hosting</span></span>
<span data-ttu-id="39ccf-104">Azure Service Fabric placerar hello power i händerna när du bestämmer du hur dina tjänster toocommunicate med användare och med varandra.</span><span class="sxs-lookup"><span data-stu-id="39ccf-104">Azure Service Fabric puts hello power in your hands when you're deciding how you want your services toocommunicate with users and with each other.</span></span> <span data-ttu-id="39ccf-105">Den här självstudiekursen fokuserar på implementering av kommunikation med Open Web Interface för .NET (OWIN) värd själv i Service Fabric Reliable Services API ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="39ccf-105">This tutorial focuses on implementing service communication using ASP.NET Web API with Open Web Interface for .NET (OWIN) self-hosting in Service Fabric's Reliable Services API.</span></span> <span data-ttu-id="39ccf-106">Vi kommer detaljerad information om djupt hello Reliable Services pluggable kommunikation API.</span><span class="sxs-lookup"><span data-stu-id="39ccf-106">We'll delve deeply into hello Reliable Services pluggable communication API.</span></span> <span data-ttu-id="39ccf-107">Vi använder också Web API i en detaljerade tooshow du hur tooset upp en anpassad kommunikation lyssnare.</span><span class="sxs-lookup"><span data-stu-id="39ccf-107">We'll also use Web API in a step-by-step example tooshow you how tooset up a custom communication listener.</span></span>

## <a name="introduction-tooweb-api-in-service-fabric"></a><span data-ttu-id="39ccf-108">Introduktion tooWeb API i Service Fabric</span><span class="sxs-lookup"><span data-stu-id="39ccf-108">Introduction tooWeb API in Service Fabric</span></span>
<span data-ttu-id="39ccf-109">ASP.NET Web API är ett populärt och kraftfullt ramverk för att skapa HTTP APIs ovanpå hello .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="39ccf-109">ASP.NET Web API is a popular and powerful framework for building HTTP APIs on top of hello .NET Framework.</span></span> <span data-ttu-id="39ccf-110">Om du inte redan är bekant med hello framework, se [komma igång med ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="39ccf-110">If you're not already familiar with hello framework, see [Getting started with ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) toolearn more.</span></span>

<span data-ttu-id="39ccf-111">Webb-API i Service Fabric är hello samma ASP.NET Web API du känner till och.</span><span class="sxs-lookup"><span data-stu-id="39ccf-111">Web API in Service Fabric is hello same ASP.NET Web API you know and love.</span></span> <span data-ttu-id="39ccf-112">hello skillnaden är i hur du *värden* ett Web API-program.</span><span class="sxs-lookup"><span data-stu-id="39ccf-112">hello difference is in how you *host* a Web API application.</span></span> <span data-ttu-id="39ccf-113">Du kommer inte att använda Microsoft Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="39ccf-113">You won't be using Microsoft Internet Information Services (IIS).</span></span> <span data-ttu-id="39ccf-114">toobetter förstå hello skillnaden, dela vi i två delar:</span><span class="sxs-lookup"><span data-stu-id="39ccf-114">toobetter understand hello difference, let's break it into two parts:</span></span>

1. <span data-ttu-id="39ccf-115">hello Web API-program (inklusive domänkontrollanter och modeller)</span><span class="sxs-lookup"><span data-stu-id="39ccf-115">hello Web API application (including controllers and models)</span></span>
2. <span data-ttu-id="39ccf-116">hello-värden (hello webbserver, vanligtvis IIS)</span><span class="sxs-lookup"><span data-stu-id="39ccf-116">hello host (hello web server, usually IIS)</span></span>

<span data-ttu-id="39ccf-117">Ett Web API-program ändras inte.</span><span class="sxs-lookup"><span data-stu-id="39ccf-117">A Web API application itself doesn't change.</span></span> <span data-ttu-id="39ccf-118">Det är inte detsamma Web API-program som du har skrivit hello senaste och bör du kunna toosimply flytta över de flesta av programkoden.</span><span class="sxs-lookup"><span data-stu-id="39ccf-118">It's no different from Web API applications you may have written in hello past, and you should be able toosimply move over most of your application code.</span></span> <span data-ttu-id="39ccf-119">Men om du har värd i IIS, där du vara värd för programmet hello kanske skiljer sig något från vad du är van vid.</span><span class="sxs-lookup"><span data-stu-id="39ccf-119">But if you've been hosting on IIS, where you host hello application may be a little different from what you're used to.</span></span> <span data-ttu-id="39ccf-120">Innan vi hämta toohello som värd för en del vi börjar med något mer bekant: hello Web API-program.</span><span class="sxs-lookup"><span data-stu-id="39ccf-120">Before we get toohello hosting part, let's start with something more familiar: hello Web API application.</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="39ccf-121">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="39ccf-121">Create hello application</span></span>
<span data-ttu-id="39ccf-122">Börja med att skapa ett nytt Service Fabric-program med en enda tillståndslös tjänst i Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="39ccf-122">Start by creating a new Service Fabric application with a single stateless service in Visual Studio 2015.</span></span>

<span data-ttu-id="39ccf-123">Visual Studio-mall för en tillståndslös tjänst med hjälp av Web API är tillgängliga tooyou.</span><span class="sxs-lookup"><span data-stu-id="39ccf-123">A Visual Studio template for a stateless service using Web API is available tooyou.</span></span> <span data-ttu-id="39ccf-124">I den här kursen ska vi skapa ett Web API-projekt från grunden som resulterar i vad du skulle få om du har valt den här mallen.</span><span class="sxs-lookup"><span data-stu-id="39ccf-124">In this tutorial, we'll build a Web API project from scratch that results in what you'd get if you selected this template.</span></span>

<span data-ttu-id="39ccf-125">Välj en tom tillståndslösa tjänsten projektet toolearn hur toobuild Web API-projekt från grunden eller du kan börja med hello tillståndslösa tjänsten Web API-mallen och bara följa med.</span><span class="sxs-lookup"><span data-stu-id="39ccf-125">Select a blank Stateless Service project toolearn how toobuild a Web API project from scratch, or you can start with hello stateless service Web API template and simply follow along.</span></span>  

<span data-ttu-id="39ccf-126">hello första steget är toopull i vissa NuGet-paket för Web API.</span><span class="sxs-lookup"><span data-stu-id="39ccf-126">hello first step is toopull in some NuGet packages for Web API.</span></span> <span data-ttu-id="39ccf-127">hello-paket som vi vill toouse är Microsoft.AspNet.WebApi.OwinSelfHost.</span><span class="sxs-lookup"><span data-stu-id="39ccf-127">hello package we want toouse is Microsoft.AspNet.WebApi.OwinSelfHost.</span></span> <span data-ttu-id="39ccf-128">Det här paketet innehåller alla hello nödvändiga Web API-paket och hello *värden* paket.</span><span class="sxs-lookup"><span data-stu-id="39ccf-128">This package includes all hello necessary Web API packages and hello *host* packages.</span></span> <span data-ttu-id="39ccf-129">Det här är viktigt senare.</span><span class="sxs-lookup"><span data-stu-id="39ccf-129">This will be important later.</span></span>

<span data-ttu-id="39ccf-130">När du har installerat hello-paket, kan du börja bygga ut hello Web API-projekt grundstrukturen.</span><span class="sxs-lookup"><span data-stu-id="39ccf-130">After hello packages have been installed, you can begin building out hello basic Web API project structure.</span></span> <span data-ttu-id="39ccf-131">Om du har använt Web API hello projektstruktur bör mycket Se bekant ut.</span><span class="sxs-lookup"><span data-stu-id="39ccf-131">If you've used Web API, hello project structure should look very familiar.</span></span> <span data-ttu-id="39ccf-132">Starta genom att lägga till en `Controllers` katalog och en domänkontrollant med enkla värden:</span><span class="sxs-lookup"><span data-stu-id="39ccf-132">Start by adding a `Controllers` directory and a simple values controller:</span></span>

<span data-ttu-id="39ccf-133">**ValuesController.cs**</span><span class="sxs-lookup"><span data-stu-id="39ccf-133">**ValuesController.cs**</span></span>

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

<span data-ttu-id="39ccf-134">Sedan lägger till en startklass på hello projektets rot tooregister hello routning, formaterare och andra konfigurationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="39ccf-134">Next, add a Startup class at hello project root tooregister hello routing, formatters, and any other configuration setup.</span></span> <span data-ttu-id="39ccf-135">Det är också där Web API ansluts toohello *värden*, som ska granskas igen senare.</span><span class="sxs-lookup"><span data-stu-id="39ccf-135">This is also where Web API plugs in toohello *host*, which will be revisited again later.</span></span> 

<span data-ttu-id="39ccf-136">**Startup.CS**</span><span class="sxs-lookup"><span data-stu-id="39ccf-136">**Startup.cs**</span></span>

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

<span data-ttu-id="39ccf-137">Det stämmer för programmet hello.</span><span class="sxs-lookup"><span data-stu-id="39ccf-137">That's it for hello application part.</span></span> <span data-ttu-id="39ccf-138">Nu har vi skapat bara hello grundläggande Web API-projekt layouten.</span><span class="sxs-lookup"><span data-stu-id="39ccf-138">At this point, we've set up just hello basic Web API project layout.</span></span> <span data-ttu-id="39ccf-139">Hittills har det bör inte mycket se annorlunda ut från Web API-projekt som du har skrivit hello senaste eller hello grundläggande Web API-mall.</span><span class="sxs-lookup"><span data-stu-id="39ccf-139">So far, it shouldn't look much different from Web API projects you may have written in hello past or from hello basic Web API template.</span></span> <span data-ttu-id="39ccf-140">Affärslogik försätts i hello domänkontrollanter och modeller som vanligt.</span><span class="sxs-lookup"><span data-stu-id="39ccf-140">Your business logic goes in hello controllers and models as usual.</span></span>

<span data-ttu-id="39ccf-141">Nu vad vi gör om värd så att vi kan faktiskt kör den?</span><span class="sxs-lookup"><span data-stu-id="39ccf-141">Now what do we do about hosting so that we can actually run it?</span></span>

## <a name="service-hosting"></a><span data-ttu-id="39ccf-142">Tjänstevärden</span><span class="sxs-lookup"><span data-stu-id="39ccf-142">Service hosting</span></span>
<span data-ttu-id="39ccf-143">I Service Fabric-tjänsten körs i en *tjänsten värdprocess*, en körbar fil som körs koden för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="39ccf-143">In Service Fabric, your service runs in a *service host process*, an executable file that runs your service code.</span></span> <span data-ttu-id="39ccf-144">När du skriver en tjänst med hjälp av hello Reliable Services API kompilerar tjänstens projekt bara tooan körbar fil som registrerar service-typen och kör din kod.</span><span class="sxs-lookup"><span data-stu-id="39ccf-144">When you write a service by using hello Reliable Services API, your service project just compiles tooan executable file that registers your service type and runs your code.</span></span> <span data-ttu-id="39ccf-145">Detta gäller i de flesta fall när du skriver en tjänst på Service Fabric i .NET.</span><span class="sxs-lookup"><span data-stu-id="39ccf-145">This is true in most cases when you write a service on Service Fabric in .NET.</span></span> <span data-ttu-id="39ccf-146">När du öppnar Program.cs i projektet för hello tillståndslösa tjänsten bör du se:</span><span class="sxs-lookup"><span data-stu-id="39ccf-146">When you open Program.cs in hello stateless service project, you should see:</span></span>

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

<span data-ttu-id="39ccf-147">Om du som ser misstänkt ut hello post punkt tooa konsolprogram som är eftersom den är.</span><span class="sxs-lookup"><span data-stu-id="39ccf-147">If that looks suspiciously like hello entry point tooa console application, that's because it is.</span></span>

<span data-ttu-id="39ccf-148">Mer information om hello serverprocess och tjänstregistrering ligger utanför hello omfånget för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="39ccf-148">Further details about hello service host process and service registration are beyond hello scope of this article.</span></span> <span data-ttu-id="39ccf-149">Men det är viktigt tooknow för nu *service koden körs i sin egen process*.</span><span class="sxs-lookup"><span data-stu-id="39ccf-149">But it's important tooknow for now that *your service code is running in its own process*.</span></span>

## <a name="self-host-web-api-with-an-owin-host"></a><span data-ttu-id="39ccf-150">Automatisk värd för webb-API med en OWIN-värd</span><span class="sxs-lookup"><span data-stu-id="39ccf-150">Self-host Web API with an OWIN host</span></span>
<span data-ttu-id="39ccf-151">Med tanke på att programkoden webb-API finns i en egen process, hur du koppla den tooa webbserver?</span><span class="sxs-lookup"><span data-stu-id="39ccf-151">Given that your Web API application code is hosted in its own process, how do you hook it up tooa web server?</span></span> <span data-ttu-id="39ccf-152">Ange [OWIN](http://owin.org/).</span><span class="sxs-lookup"><span data-stu-id="39ccf-152">Enter [OWIN](http://owin.org/).</span></span> <span data-ttu-id="39ccf-153">OWIN är bara ett avtal mellan .NET-webbprogram och webbservrar.</span><span class="sxs-lookup"><span data-stu-id="39ccf-153">OWIN is simply a contract between .NET web applications and web servers.</span></span> <span data-ttu-id="39ccf-154">När ASP.NET (upp tooMVC 5) används är traditionellt tätt kopplade tooIIS via System.Web hello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="39ccf-154">Traditionally when ASP.NET (up tooMVC 5) is used, hello web application is tightly coupled tooIIS through System.Web.</span></span> <span data-ttu-id="39ccf-155">Men implementerar Web API OWIN, så du kan skriva ett program som är fristående från hello-webbserver som är värd för den.</span><span class="sxs-lookup"><span data-stu-id="39ccf-155">However, Web API implements OWIN, so you can write a web application that is decoupled from hello web server that hosts it.</span></span> <span data-ttu-id="39ccf-156">Därför kan du använda en *egenvärdbaserat* OWIN-webbserver som du kan starta i en egen process.</span><span class="sxs-lookup"><span data-stu-id="39ccf-156">Because of this, you can use a *self-hosted* OWIN web server that you can start in your own process.</span></span> <span data-ttu-id="39ccf-157">Detta passar perfekt med hello Service Fabric värdmodell vi bara beskrivs.</span><span class="sxs-lookup"><span data-stu-id="39ccf-157">This fits perfectly with hello Service Fabric hosting model we just described.</span></span>

<span data-ttu-id="39ccf-158">I den här artikeln använder vi Katana som hello OWIN-värd för hello Web API-program.</span><span class="sxs-lookup"><span data-stu-id="39ccf-158">In this article, we'll use Katana as hello OWIN host for hello Web API application.</span></span> <span data-ttu-id="39ccf-159">Katana är en öppen källkod OWIN värd-implementering som bygger på [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) och hello Windows [http-Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="39ccf-159">Katana is an open-source OWIN host implementation built on [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) and hello Windows [HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="39ccf-160">Mer information om Katana, gå toohello toolearn [Katana plats](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana).</span><span class="sxs-lookup"><span data-stu-id="39ccf-160">toolearn more about Katana, go toohello [Katana site](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana).</span></span> <span data-ttu-id="39ccf-161">För en snabb överblick över hur toouse Katana tooself värden Web API finns [Använd OWIN tooSelf värden ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).</span><span class="sxs-lookup"><span data-stu-id="39ccf-161">For a quick overview of how toouse Katana tooself-host Web API, see [Use OWIN tooSelf-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).</span></span>
> 
> 

## <a name="set-up-hello-web-server"></a><span data-ttu-id="39ccf-162">Konfigurera hello webbserver</span><span class="sxs-lookup"><span data-stu-id="39ccf-162">Set up hello web server</span></span>
<span data-ttu-id="39ccf-163">hello Reliable Services API ger en startpunkt för kommunikation där du kan ansluta i kommunikation staplar som gör att användare och klienter tooconnect toohello tjänsten:</span><span class="sxs-lookup"><span data-stu-id="39ccf-163">hello Reliable Services API provides a communication entry point where you can plug in communication stacks that allow users and clients tooconnect toohello service:</span></span>

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

<span data-ttu-id="39ccf-164">hello webbserver (och eventuella andra kommunikation stack du använder i hello framtida, till exempel WebSockets) ska använda hello ICommunicationListener gränssnittet toointegrate korrekt med hello system.</span><span class="sxs-lookup"><span data-stu-id="39ccf-164">hello web server (and any other communication stack you use in hello future, such as WebSockets) should use hello ICommunicationListener interface toointegrate correctly with hello system.</span></span> <span data-ttu-id="39ccf-165">hello orsaker till detta kommer att bli tydligare i hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="39ccf-165">hello reasons for this will become more apparent in hello following steps.</span></span>

<span data-ttu-id="39ccf-166">Först skapar du en klass med namnet OwinCommunicationListener som implementerar ICommunicationListener:</span><span class="sxs-lookup"><span data-stu-id="39ccf-166">First, create a class called OwinCommunicationListener that implements ICommunicationListener:</span></span>

<span data-ttu-id="39ccf-167">**OwinCommunicationListener.cs**</span><span class="sxs-lookup"><span data-stu-id="39ccf-167">**OwinCommunicationListener.cs**</span></span>

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

<span data-ttu-id="39ccf-168">Hej ICommunicationListener gränssnittet innehåller tre metoder toomanage en lyssnare för kommunikation för din tjänst:</span><span class="sxs-lookup"><span data-stu-id="39ccf-168">hello ICommunicationListener interface provides three methods toomanage a communication listener for your service:</span></span>

* <span data-ttu-id="39ccf-169">*OpenAsync*.</span><span class="sxs-lookup"><span data-stu-id="39ccf-169">*OpenAsync*.</span></span> <span data-ttu-id="39ccf-170">Börja lyssna efter begäranden.</span><span class="sxs-lookup"><span data-stu-id="39ccf-170">Start listening for requests.</span></span>
* <span data-ttu-id="39ccf-171">*CloseAsync*.</span><span class="sxs-lookup"><span data-stu-id="39ccf-171">*CloseAsync*.</span></span> <span data-ttu-id="39ccf-172">Slutar att lyssna efter begäranden, Slutför alla pågående begäranden och avslutas på vanligt sätt.</span><span class="sxs-lookup"><span data-stu-id="39ccf-172">Stop listening for requests, finish any in-flight requests, and shut down gracefully.</span></span>
* <span data-ttu-id="39ccf-173">*Avbryt*.</span><span class="sxs-lookup"><span data-stu-id="39ccf-173">*Abort*.</span></span> <span data-ttu-id="39ccf-174">Avbryt allt och stoppa omedelbart.</span><span class="sxs-lookup"><span data-stu-id="39ccf-174">Cancel everything and stop immediately.</span></span>

<span data-ttu-id="39ccf-175">tooget igång, lägga till privata klassmedlemmar för saker hello lyssnare måste toofunction.</span><span class="sxs-lookup"><span data-stu-id="39ccf-175">tooget started, add private class members for things hello listener will need toofunction.</span></span> <span data-ttu-id="39ccf-176">Dessa initieras via hello konstruktorn och användas senare när du ställer in hello lyssnar URL.</span><span class="sxs-lookup"><span data-stu-id="39ccf-176">These will be initialized through hello constructor and used later when you set up hello listening URL.</span></span>

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

## <a name="implement-openasync"></a><span data-ttu-id="39ccf-177">Implementera OpenAsync</span><span class="sxs-lookup"><span data-stu-id="39ccf-177">Implement OpenAsync</span></span>
<span data-ttu-id="39ccf-178">tooset in hello webbserver, behöver du två typer av information:</span><span class="sxs-lookup"><span data-stu-id="39ccf-178">tooset up hello web server, you need two pieces of information:</span></span>

* <span data-ttu-id="39ccf-179">*Ett URL-prefix för sökvägen*.</span><span class="sxs-lookup"><span data-stu-id="39ccf-179">*A URL path prefix*.</span></span> <span data-ttu-id="39ccf-180">Även om det här är valfritt, är det bra om du tooset detta nu upp så att du kan på ett säkert sätt vara värd för flera webbtjänster i ditt program.</span><span class="sxs-lookup"><span data-stu-id="39ccf-180">Although this is optional, it's good for you tooset this up now so that you can safely host multiple web services in your application.</span></span>
* <span data-ttu-id="39ccf-181">*En port*.</span><span class="sxs-lookup"><span data-stu-id="39ccf-181">*A port*.</span></span>

<span data-ttu-id="39ccf-182">Det är viktigt att du förstår att Service Fabric ger ett lager för program som fungerar som en buffert mellan programmet och hello underliggande operativsystemet som körs på innan du får en port för hello webbservern.</span><span class="sxs-lookup"><span data-stu-id="39ccf-182">Before you get a port for hello web server, it's important that you understand that Service Fabric provides an application layer that acts as a buffer between your application and hello underlying operating system that it runs on.</span></span> <span data-ttu-id="39ccf-183">Service Fabric tillhandahåller en sätt tooconfigure *slutpunkter* för dina tjänster.</span><span class="sxs-lookup"><span data-stu-id="39ccf-183">As such, Service Fabric provides a way tooconfigure *endpoints* for your services.</span></span> <span data-ttu-id="39ccf-184">Service Fabric säkerställer att slutpunkter är tillgängliga för din tjänst toouse.</span><span class="sxs-lookup"><span data-stu-id="39ccf-184">Service Fabric ensures that endpoints are available for your service toouse.</span></span> <span data-ttu-id="39ccf-185">På så sätt kan du inte har tooconfigure dem själv i hello underliggande OS-miljö.</span><span class="sxs-lookup"><span data-stu-id="39ccf-185">This way, you don't have tooconfigure them yourself in hello underlying OS environment.</span></span> <span data-ttu-id="39ccf-186">Du kan enkelt värd Service Fabric programmet i olika miljöer utan att behöva toomake alla ändringar tooyour program.</span><span class="sxs-lookup"><span data-stu-id="39ccf-186">You can easily host your Service Fabric application in different environments without having toomake any changes tooyour application.</span></span> <span data-ttu-id="39ccf-187">(Du kan till exempel värd hello samma program i Azure eller i ditt eget datacenter.)</span><span class="sxs-lookup"><span data-stu-id="39ccf-187">(For example, you can host hello same application in Azure or in your own datacenter.)</span></span>

<span data-ttu-id="39ccf-188">Konfigurera en HTTP-slutpunkt i PackageRoot\ServiceManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="39ccf-188">Configure an HTTP endpoint in PackageRoot\ServiceManifest.xml:</span></span>

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

<span data-ttu-id="39ccf-189">Det här steget är viktigt eftersom hello värdprocessen för service körs under autentiseringsuppgifterna för begränsade (Network Service i Windows).</span><span class="sxs-lookup"><span data-stu-id="39ccf-189">This step is important because hello service host process runs under restricted credentials (Network Service on Windows).</span></span> <span data-ttu-id="39ccf-190">Det innebär att tjänsten inte har åtkomst tooset in en HTTP-slutpunkt på sin egen.</span><span class="sxs-lookup"><span data-stu-id="39ccf-190">This means that your service won't have access tooset up an HTTP endpoint on its own.</span></span> <span data-ttu-id="39ccf-191">Med hjälp av hello slutpunktskonfiguration vet Service Fabric tooset in hello rätt åtkomstkontrollista (ACL) för hello URL som hello tjänsten lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="39ccf-191">By using hello endpoint configuration, Service Fabric knows tooset up hello proper access control list (ACL) for hello URL that hello service will listen on.</span></span> <span data-ttu-id="39ccf-192">Service Fabric innehåller också en standardplats tooconfigure slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="39ccf-192">Service Fabric also provides a standard place tooconfigure endpoints.</span></span>

<span data-ttu-id="39ccf-193">Tillbaka i OwinCommunicationListener.cs börjar du implementera OpenAsync.</span><span class="sxs-lookup"><span data-stu-id="39ccf-193">Back in OwinCommunicationListener.cs, you can start implementing OpenAsync.</span></span> <span data-ttu-id="39ccf-194">Det är där du startar hello webbservern.</span><span class="sxs-lookup"><span data-stu-id="39ccf-194">This is where you start hello web server.</span></span> <span data-ttu-id="39ccf-195">Först hämta hello slutpunktsinformation och skapa hello-URL som hello tjänsten lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="39ccf-195">First, get hello endpoint information and create hello URL that hello service will listen on.</span></span> <span data-ttu-id="39ccf-196">hello URL är olika beroende på om hello lyssnare används i en tillståndslös tjänst eller en tillståndskänslig service.</span><span class="sxs-lookup"><span data-stu-id="39ccf-196">hello URL will be different depending on whether hello listener is used in a stateless service or a stateful service.</span></span> <span data-ttu-id="39ccf-197">För en tillståndskänslig service måste hello lyssnare toocreate en unik adress för varje replik tillståndskänslig service den lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="39ccf-197">For a stateful service, hello listener needs toocreate a unique address for every stateful service replica it listens on.</span></span> <span data-ttu-id="39ccf-198">Hello-adressen kan vara mycket enklare för tillståndslösa tjänster.</span><span class="sxs-lookup"><span data-stu-id="39ccf-198">For stateless services, hello address can be much simpler.</span></span> 

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

<span data-ttu-id="39ccf-199">Observera att ”http://+” användas här.</span><span class="sxs-lookup"><span data-stu-id="39ccf-199">Note that "http://+" is used here.</span></span> <span data-ttu-id="39ccf-200">Detta är toomake till webbservern hello lyssnar på alla tillgängliga adresser, inklusive localhost, FQDN och hello dator-IP.</span><span class="sxs-lookup"><span data-stu-id="39ccf-200">This is toomake sure that hello web server is listening on all available addresses, including localhost, FQDN, and hello machine IP.</span></span>

<span data-ttu-id="39ccf-201">hello OpenAsync implementering är ett av hello viktigaste skälen varför hello webbserver (eller någon kommunikation stack) implementeras som en ICommunicationListener, istället för bara den öppna direkt från `RunAsync()` i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="39ccf-201">hello OpenAsync implementation is one of hello most important reasons why hello web server (or any communication stack) is implemented as an ICommunicationListener, rather than just having it open directly from `RunAsync()` in hello service.</span></span> <span data-ttu-id="39ccf-202">hello returvärde från OpenAsync är hello-adress som hello webbservern lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="39ccf-202">hello return value from OpenAsync is hello address that hello web server is listening on.</span></span> <span data-ttu-id="39ccf-203">När den här adressen returneras toohello system, registrerar hello-adress med hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="39ccf-203">When this address is returned toohello system, it registers hello address with hello service.</span></span> <span data-ttu-id="39ccf-204">Service Fabric tillhandahåller ett API som gör att klienter och andra tjänster toothen fråga för den här adressen av tjänstnamn.</span><span class="sxs-lookup"><span data-stu-id="39ccf-204">Service Fabric provides an API that allows clients and other services toothen ask for this address by service name.</span></span> <span data-ttu-id="39ccf-205">Detta är viktigt eftersom hello-adress inte är statisk.</span><span class="sxs-lookup"><span data-stu-id="39ccf-205">This is important because hello service address is not static.</span></span> <span data-ttu-id="39ccf-206">Tjänster flyttas i hello kluster för resursen belastningsutjämning och tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="39ccf-206">Services are moved around in hello cluster for resource balancing and availability purposes.</span></span> <span data-ttu-id="39ccf-207">Detta är hello mekanism som gör att klienter tooresolve hello lyssnar-adress för en tjänst.</span><span class="sxs-lookup"><span data-stu-id="39ccf-207">This is hello mechanism that allows clients tooresolve hello listening address for a service.</span></span>

<span data-ttu-id="39ccf-208">Med detta i åtanke OpenAsync startar hello webbservern och returnerar hello-adressen som lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="39ccf-208">With that in mind, OpenAsync starts hello web server and returns hello address that it's listening on.</span></span> <span data-ttu-id="39ccf-209">Observera att den lyssnar på ”http://+”, men innan OpenAsync returnerar hello adress hello ”+” ersätts med hello IP eller FQDN för hello nod den för närvarande finns i.</span><span class="sxs-lookup"><span data-stu-id="39ccf-209">Note that it listens on "http://+", but before OpenAsync returns hello address, hello "+" is replaced with hello IP or FQDN of hello node it is currently on.</span></span> <span data-ttu-id="39ccf-210">hello-adressen som returneras av den här metoden är vad som har registrerats med hello system.</span><span class="sxs-lookup"><span data-stu-id="39ccf-210">hello address that is returned by this method is what's registered with hello system.</span></span> <span data-ttu-id="39ccf-211">Det är också klienter och andra tjänster ser när de frågar efter en tjänst-adress.</span><span class="sxs-lookup"><span data-stu-id="39ccf-211">It's also what clients and other services see when they ask for a service's address.</span></span> <span data-ttu-id="39ccf-212">För klienter toocorrectly ansluta tooit, de behöver en faktiska IP eller FQDN i hello-adress.</span><span class="sxs-lookup"><span data-stu-id="39ccf-212">For clients toocorrectly connect tooit, they need an actual IP or FQDN in hello address.</span></span>

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

<span data-ttu-id="39ccf-213">Observera att detta refererar hello-startklass som skickades i toohello OwinCommunicationListener i hello konstruktor.</span><span class="sxs-lookup"><span data-stu-id="39ccf-213">Note that this references hello Startup class that was passed in toohello OwinCommunicationListener in hello constructor.</span></span> <span data-ttu-id="39ccf-214">Den här Start-instansen används av hello web server toobootstrap hello Web API-program.</span><span class="sxs-lookup"><span data-stu-id="39ccf-214">This startup instance is used by hello web server toobootstrap hello Web API application.</span></span>

<span data-ttu-id="39ccf-215">Hej `ServiceEventSource.Current.Message()` raden visas i fönstret för hello diagnostiska händelser senare, när du kör hello programmet tooconfirm hello webbservern har startats.</span><span class="sxs-lookup"><span data-stu-id="39ccf-215">hello `ServiceEventSource.Current.Message()` line will appear in hello Diagnostic Events window later, when you run hello application tooconfirm that hello web server has started successfully.</span></span>

## <a name="implement-closeasync-and-abort"></a><span data-ttu-id="39ccf-216">Implementera CloseAsync och ITransaction::Abort</span><span class="sxs-lookup"><span data-stu-id="39ccf-216">Implement CloseAsync and Abort</span></span>
<span data-ttu-id="39ccf-217">Slutligen implementerar både CloseAsync och Avbryt toostop hello-webbserver.</span><span class="sxs-lookup"><span data-stu-id="39ccf-217">Finally, implement both CloseAsync and Abort toostop hello web server.</span></span> <span data-ttu-id="39ccf-218">hello webbservern kan stoppas av avyttring hello server hantera som skapades under OpenAsync.</span><span class="sxs-lookup"><span data-stu-id="39ccf-218">hello web server can be stopped by disposing hello server handle that was created during OpenAsync.</span></span>

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

<span data-ttu-id="39ccf-219">I det här implementeringsexemplet stoppa både CloseAsync och Avbryt bara hello webbserver.</span><span class="sxs-lookup"><span data-stu-id="39ccf-219">In this implementation example, both CloseAsync and Abort simply stop hello web server.</span></span> <span data-ttu-id="39ccf-220">Du kan välja tooperform mer smidigt samordnade stängas av hello webbserver i CloseAsync.</span><span class="sxs-lookup"><span data-stu-id="39ccf-220">You may opt tooperform a more gracefully coordinated shutdown of hello web server in CloseAsync.</span></span> <span data-ttu-id="39ccf-221">Hello avstängning kan vänta pågående begäranden toobe slutförts innan hello returnerar.</span><span class="sxs-lookup"><span data-stu-id="39ccf-221">For example, hello shutdown could wait for in-flight requests toobe completed before hello return.</span></span>

## <a name="start-hello-web-server"></a><span data-ttu-id="39ccf-222">Starta hello webbserver</span><span class="sxs-lookup"><span data-stu-id="39ccf-222">Start hello web server</span></span>
<span data-ttu-id="39ccf-223">Du är nu redo toocreate och returnera en instans av OwinCommunicationListener toostart hello-webbserver.</span><span class="sxs-lookup"><span data-stu-id="39ccf-223">You're now ready toocreate and return an instance of OwinCommunicationListener toostart hello web server.</span></span> <span data-ttu-id="39ccf-224">Tillbaka i hello tjänstklass (WebService.cs), åsidosätta hello `CreateServiceInstanceListeners()` metoden:</span><span class="sxs-lookup"><span data-stu-id="39ccf-224">Back in hello Service class (WebService.cs), override hello `CreateServiceInstanceListeners()` method:</span></span>

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

<span data-ttu-id="39ccf-225">Det är där hello Web API *programmet* och hello OWIN *värden* slutligen uppfyller.</span><span class="sxs-lookup"><span data-stu-id="39ccf-225">This is where hello Web API *application* and hello OWIN *host* finally meet.</span></span> <span data-ttu-id="39ccf-226">hello-värden (OwinCommunicationListener) ges en instans av hello *programmet* (webb-API) via hello-startklass.</span><span class="sxs-lookup"><span data-stu-id="39ccf-226">hello host (OwinCommunicationListener) is given an instance of hello *application* (Web API) via hello Startup class.</span></span> <span data-ttu-id="39ccf-227">Service Fabric hanterar sedan dess livscykel.</span><span class="sxs-lookup"><span data-stu-id="39ccf-227">Service Fabric then manages its lifecycle.</span></span> <span data-ttu-id="39ccf-228">Den här samma mönster kan vanligtvis följas med någon kommunikation stack.</span><span class="sxs-lookup"><span data-stu-id="39ccf-228">This same pattern can typically be followed with any communication stack.</span></span>

## <a name="put-it-all-together"></a><span data-ttu-id="39ccf-229">Färdigställa allt</span><span class="sxs-lookup"><span data-stu-id="39ccf-229">Put it all together</span></span>
<span data-ttu-id="39ccf-230">I det här exemplet behöver du inte toodo allt annat i hello `RunAsync()` metod, så att åsidosättningen kan bara tas bort.</span><span class="sxs-lookup"><span data-stu-id="39ccf-230">In this example, you don't need toodo anything in hello `RunAsync()` method, so that override can simply be removed.</span></span>

<span data-ttu-id="39ccf-231">hello slutliga implementering ska vara mycket enkelt.</span><span class="sxs-lookup"><span data-stu-id="39ccf-231">hello final service implementation should be very simple.</span></span> <span data-ttu-id="39ccf-232">Det bara behöver toocreate hello kommunikation lyssnare:</span><span class="sxs-lookup"><span data-stu-id="39ccf-232">It only needs toocreate hello communication listener:</span></span>

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

<span data-ttu-id="39ccf-233">hello fullständig `OwinCommunicationListener` klass:</span><span class="sxs-lookup"><span data-stu-id="39ccf-233">hello complete `OwinCommunicationListener` class:</span></span>

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

<span data-ttu-id="39ccf-234">Nu när du har placerat alla hello delar på plats, ditt projekt bör se ut som ett typiskt Web API-program med tillförlitlig Services API startpunkter och en OWIN-värd.</span><span class="sxs-lookup"><span data-stu-id="39ccf-234">Now that you have put all hello pieces in place, your project should look like a typical Web API application with Reliable Services API entry points and an OWIN host.</span></span>

## <a name="run-and-connect-through-a-web-browser"></a><span data-ttu-id="39ccf-235">Kör och ansluter via en webbläsare</span><span class="sxs-lookup"><span data-stu-id="39ccf-235">Run and connect through a web browser</span></span>
<span data-ttu-id="39ccf-236">Om du inte gjort det, [ställa in din utvecklingsmiljö](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="39ccf-236">If you haven't done so, [set up your development environment](service-fabric-get-started.md).</span></span>

<span data-ttu-id="39ccf-237">Du kan nu skapa och distribuera din tjänst.</span><span class="sxs-lookup"><span data-stu-id="39ccf-237">You can now build and deploy your service.</span></span> <span data-ttu-id="39ccf-238">Tryck på **F5** i Visual Studio toobuild och distribuera hello program.</span><span class="sxs-lookup"><span data-stu-id="39ccf-238">Press **F5** in Visual Studio toobuild and deploy hello application.</span></span> <span data-ttu-id="39ccf-239">I hello diagnostiska händelser fönster bör du se ett meddelande som anger hello webbservern öppnas på http://localhost:8281 /.</span><span class="sxs-lookup"><span data-stu-id="39ccf-239">In hello Diagnostic Events window, you should see a message that indicates that hello web server opened on http://localhost:8281/.</span></span>

> [!NOTE]
> <span data-ttu-id="39ccf-240">Om hello port har redan öppnats av någon annan process på datorn, visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="39ccf-240">If hello port has already been opened by another process on your machine, you may see an error here.</span></span> <span data-ttu-id="39ccf-241">Detta anger att hello-lyssnaren inte kunde öppnas.</span><span class="sxs-lookup"><span data-stu-id="39ccf-241">This indicates that hello listener couldn't be opened.</span></span> <span data-ttu-id="39ccf-242">Om så är fallet hello kan du prova att använda en annan port för hello slutpunktskonfiguration i ServiceManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="39ccf-242">If that's hello case, try using a different port for hello endpoint configuration in ServiceManifest.xml.</span></span>
> 
> 

<span data-ttu-id="39ccf-243">När hello-tjänsten körs, öppna en webbläsare och gå för[http://localhost:8281-api-värden](http://localhost:8281/api/values) tootest den.</span><span class="sxs-lookup"><span data-stu-id="39ccf-243">Once hello service is running, open a browser and navigate too[http://localhost:8281/api/values](http://localhost:8281/api/values) tootest it.</span></span>

## <a name="scale-it-out"></a><span data-ttu-id="39ccf-244">Skala ut</span><span class="sxs-lookup"><span data-stu-id="39ccf-244">Scale it out</span></span>
<span data-ttu-id="39ccf-245">Skala ut tillståndslös webbprogram vanligtvis innebär att lägga till flera datorer och igång hello webbprogram på dem..</span><span class="sxs-lookup"><span data-stu-id="39ccf-245">Scaling out stateless web apps typically means adding more machines and spinning up hello web apps on them.</span></span> <span data-ttu-id="39ccf-246">Service Fabric orchestration-motorn kan göra när nya noder läggs tooa klustret.</span><span class="sxs-lookup"><span data-stu-id="39ccf-246">Service Fabric's orchestration engine can do this for you whenever new nodes are added tooa cluster.</span></span> <span data-ttu-id="39ccf-247">När du skapar instanser av tillståndslösa tjänsten måste du ange hello många instanser som du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="39ccf-247">When you create instances of a stateless service, you can specify hello number of instances you want toocreate.</span></span> <span data-ttu-id="39ccf-248">Service Fabric placeras det antalet instanser på noderna i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="39ccf-248">Service Fabric places that number of instances on nodes in hello cluster.</span></span> <span data-ttu-id="39ccf-249">Och det gör att toocreate inte fler än en instans på varje nod i ett.</span><span class="sxs-lookup"><span data-stu-id="39ccf-249">And it makes sure not toocreate more than one instance on any one node.</span></span> <span data-ttu-id="39ccf-250">Du kan också instruera Service Fabric tooalways skapa en instans på varje nod genom att ange **-1** för hello instanser.</span><span class="sxs-lookup"><span data-stu-id="39ccf-250">You can also instruct Service Fabric tooalways create an instance on every node by specifying **-1** for hello instance count.</span></span> <span data-ttu-id="39ccf-251">Detta garanterar att när du lägger till tooscale noder i klustret en instans av tillståndslösa tjänsten kommer att skapas på hello nya noder.</span><span class="sxs-lookup"><span data-stu-id="39ccf-251">This guarantees that whenever you add nodes tooscale out your cluster, an instance of your stateless service will be created on hello new nodes.</span></span> <span data-ttu-id="39ccf-252">Det här värdet är en egenskap för hello tjänstinstans, så anges när du skapar en instans av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="39ccf-252">This value is a property of hello service instance, so it is set when you create a service instance.</span></span> <span data-ttu-id="39ccf-253">Du kan göra detta med hjälp av PowerShell:</span><span class="sxs-lookup"><span data-stu-id="39ccf-253">You can do this through PowerShell:</span></span>

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

<span data-ttu-id="39ccf-254">Du kan också göra detta när du definierar en standardtjänst i ett Visual Studio-projekt för tillståndslösa tjänsten:</span><span class="sxs-lookup"><span data-stu-id="39ccf-254">You can also do this when you define a default service in a Visual Studio stateless service project:</span></span>

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

<span data-ttu-id="39ccf-255">Mer information om hur toocreate program och instanser av tjänsten, se [distribuera ett program](service-fabric-deploy-remove-applications.md).</span><span class="sxs-lookup"><span data-stu-id="39ccf-255">For more information on how toocreate application and service instances, see [Deploy an application](service-fabric-deploy-remove-applications.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="39ccf-256">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="39ccf-256">Next steps</span></span>
[<span data-ttu-id="39ccf-257">Felsöka Service Fabric-program med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39ccf-257">Debug your Service Fabric application by using Visual Studio</span></span>](service-fabric-debugging-your-application.md)

