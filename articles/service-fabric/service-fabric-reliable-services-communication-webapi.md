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
# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a><span data-ttu-id="93f69-103">Komma igång: Service Fabric Web API-tjänster med OWIN själva värd</span><span class="sxs-lookup"><span data-stu-id="93f69-103">Get started: Service Fabric Web API services with OWIN self-hosting</span></span>
<span data-ttu-id="93f69-104">Azure Service Fabric placerar kraften i dina händer när du bestämmer hur du vill att dina tjänster att kommunicera med användare och med varandra.</span><span class="sxs-lookup"><span data-stu-id="93f69-104">Azure Service Fabric puts the power in your hands when you're deciding how you want your services to communicate with users and with each other.</span></span> <span data-ttu-id="93f69-105">Den här självstudiekursen fokuserar på implementering av kommunikation med Open Web Interface för .NET (OWIN) värd själv i Service Fabric Reliable Services API ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="93f69-105">This tutorial focuses on implementing service communication using ASP.NET Web API with Open Web Interface for .NET (OWIN) self-hosting in Service Fabric's Reliable Services API.</span></span> <span data-ttu-id="93f69-106">Vi kommer detaljerad information om djupt pluggable kommunikation Reliable Services API.</span><span class="sxs-lookup"><span data-stu-id="93f69-106">We'll delve deeply into the Reliable Services pluggable communication API.</span></span> <span data-ttu-id="93f69-107">Vi använder också Web API i ett steg för steg-exempel som visar hur du ställer in en anpassad kommunikation lyssnare.</span><span class="sxs-lookup"><span data-stu-id="93f69-107">We'll also use Web API in a step-by-step example to show you how to set up a custom communication listener.</span></span>

## <a name="introduction-to-web-api-in-service-fabric"></a><span data-ttu-id="93f69-108">Introduktion till webb-API i Service Fabric</span><span class="sxs-lookup"><span data-stu-id="93f69-108">Introduction to Web API in Service Fabric</span></span>
<span data-ttu-id="93f69-109">ASP.NET Web API är ett populärt och kraftfullt ramverk för att skapa HTTP APIs ovanpå .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="93f69-109">ASP.NET Web API is a popular and powerful framework for building HTTP APIs on top of the .NET Framework.</span></span> <span data-ttu-id="93f69-110">Om du inte redan är bekant med framework, se [komma igång med ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="93f69-110">If you're not already familiar with the framework, see [Getting started with ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) to learn more.</span></span>

<span data-ttu-id="93f69-111">Webb-API i Service Fabric är samma ASP.NET Web API du känner till och.</span><span class="sxs-lookup"><span data-stu-id="93f69-111">Web API in Service Fabric is the same ASP.NET Web API you know and love.</span></span> <span data-ttu-id="93f69-112">Skillnaden är i hur du *värden* ett Web API-program.</span><span class="sxs-lookup"><span data-stu-id="93f69-112">The difference is in how you *host* a Web API application.</span></span> <span data-ttu-id="93f69-113">Du kommer inte att använda Microsoft Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="93f69-113">You won't be using Microsoft Internet Information Services (IIS).</span></span> <span data-ttu-id="93f69-114">För att bättre förstå skillnaden kan vi dela i två delar:</span><span class="sxs-lookup"><span data-stu-id="93f69-114">To better understand the difference, let's break it into two parts:</span></span>

1. <span data-ttu-id="93f69-115">Web API-program (inklusive domänkontrollanter och modeller)</span><span class="sxs-lookup"><span data-stu-id="93f69-115">The Web API application (including controllers and models)</span></span>
2. <span data-ttu-id="93f69-116">Värden (webbservern, vanligtvis IIS)</span><span class="sxs-lookup"><span data-stu-id="93f69-116">The host (the web server, usually IIS)</span></span>

<span data-ttu-id="93f69-117">Ett Web API-program ändras inte.</span><span class="sxs-lookup"><span data-stu-id="93f69-117">A Web API application itself doesn't change.</span></span> <span data-ttu-id="93f69-118">Det är inte detsamma Web API-program som du har skrivit tidigare och du bör kunna helt enkelt flytta över de flesta av programkoden.</span><span class="sxs-lookup"><span data-stu-id="93f69-118">It's no different from Web API applications you may have written in the past, and you should be able to simply move over most of your application code.</span></span> <span data-ttu-id="93f69-119">Men om du värd för IIS, där värden programmet kanske skiljer sig något från vad du ofta.</span><span class="sxs-lookup"><span data-stu-id="93f69-119">But if you've been hosting on IIS, where you host the application may be a little different from what you're used to.</span></span> <span data-ttu-id="93f69-120">Innan det är dags att värd, låt oss börja med något mer bekant: Web API-program.</span><span class="sxs-lookup"><span data-stu-id="93f69-120">Before we get to the hosting part, let's start with something more familiar: the Web API application.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="93f69-121">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="93f69-121">Create the application</span></span>
<span data-ttu-id="93f69-122">Börja med att skapa ett nytt Service Fabric-program med en enda tillståndslös tjänst i Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="93f69-122">Start by creating a new Service Fabric application with a single stateless service in Visual Studio 2015.</span></span>

<span data-ttu-id="93f69-123">Visual Studio-mall för en tillståndslös tjänst med hjälp av Web API är tillgängliga för dig.</span><span class="sxs-lookup"><span data-stu-id="93f69-123">A Visual Studio template for a stateless service using Web API is available to you.</span></span> <span data-ttu-id="93f69-124">I den här kursen ska vi skapa ett Web API-projekt från grunden som resulterar i vad du skulle få om du har valt den här mallen.</span><span class="sxs-lookup"><span data-stu-id="93f69-124">In this tutorial, we'll build a Web API project from scratch that results in what you'd get if you selected this template.</span></span>

<span data-ttu-id="93f69-125">Välj ett tomt projekt tillståndslösa tjänsten att lära dig hur du skapar ett Web API-projekt från grunden eller du kan starta med hjälp av tillståndslösa tjänsten Web API-mallen och bara följa med.</span><span class="sxs-lookup"><span data-stu-id="93f69-125">Select a blank Stateless Service project to learn how to build a Web API project from scratch, or you can start with the stateless service Web API template and simply follow along.</span></span>  

<span data-ttu-id="93f69-126">Det första steget är att dra in vissa NuGet-paket för Web API.</span><span class="sxs-lookup"><span data-stu-id="93f69-126">The first step is to pull in some NuGet packages for Web API.</span></span> <span data-ttu-id="93f69-127">Vi vill använda paketet är Microsoft.AspNet.WebApi.OwinSelfHost.</span><span class="sxs-lookup"><span data-stu-id="93f69-127">The package we want to use is Microsoft.AspNet.WebApi.OwinSelfHost.</span></span> <span data-ttu-id="93f69-128">Det här paketet innehåller alla nödvändiga Web API-paket och *värden* paket.</span><span class="sxs-lookup"><span data-stu-id="93f69-128">This package includes all the necessary Web API packages and the *host* packages.</span></span> <span data-ttu-id="93f69-129">Det här är viktigt senare.</span><span class="sxs-lookup"><span data-stu-id="93f69-129">This will be important later.</span></span>

<span data-ttu-id="93f69-130">När paketen har installerats kan börja du skapa grundstrukturen för Web API-projekt.</span><span class="sxs-lookup"><span data-stu-id="93f69-130">After the packages have been installed, you can begin building out the basic Web API project structure.</span></span> <span data-ttu-id="93f69-131">Om du har använt Web API projektstrukturen bör mycket Se bekant ut.</span><span class="sxs-lookup"><span data-stu-id="93f69-131">If you've used Web API, the project structure should look very familiar.</span></span> <span data-ttu-id="93f69-132">Starta genom att lägga till en `Controllers` katalog och en domänkontrollant med enkla värden:</span><span class="sxs-lookup"><span data-stu-id="93f69-132">Start by adding a `Controllers` directory and a simple values controller:</span></span>

<span data-ttu-id="93f69-133">**ValuesController.cs**</span><span class="sxs-lookup"><span data-stu-id="93f69-133">**ValuesController.cs**</span></span>

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

<span data-ttu-id="93f69-134">Sedan lägger till en startklass i projektroten att registrera routning, formaterare och andra konfigurationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="93f69-134">Next, add a Startup class at the project root to register the routing, formatters, and any other configuration setup.</span></span> <span data-ttu-id="93f69-135">Det är också där Web API ansluts till den *värden*, som ska granskas igen senare.</span><span class="sxs-lookup"><span data-stu-id="93f69-135">This is also where Web API plugs in to the *host*, which will be revisited again later.</span></span> 

<span data-ttu-id="93f69-136">**Startup.CS**</span><span class="sxs-lookup"><span data-stu-id="93f69-136">**Startup.cs**</span></span>

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

<span data-ttu-id="93f69-137">Det stämmer för programmet del.</span><span class="sxs-lookup"><span data-stu-id="93f69-137">That's it for the application part.</span></span> <span data-ttu-id="93f69-138">Nu har vi skapat bara grundläggande Web API-Projektlayout.</span><span class="sxs-lookup"><span data-stu-id="93f69-138">At this point, we've set up just the basic Web API project layout.</span></span> <span data-ttu-id="93f69-139">Hittills har det får inte mycket se annorlunda ut från Web API-projekt som du har skrivit tidigare eller från den grundläggande Web API-mallen.</span><span class="sxs-lookup"><span data-stu-id="93f69-139">So far, it shouldn't look much different from Web API projects you may have written in the past or from the basic Web API template.</span></span> <span data-ttu-id="93f69-140">Affärslogik försätts i domänkontrollanter och modeller som vanligt.</span><span class="sxs-lookup"><span data-stu-id="93f69-140">Your business logic goes in the controllers and models as usual.</span></span>

<span data-ttu-id="93f69-141">Nu vad vi gör om värd så att vi kan faktiskt kör den?</span><span class="sxs-lookup"><span data-stu-id="93f69-141">Now what do we do about hosting so that we can actually run it?</span></span>

## <a name="service-hosting"></a><span data-ttu-id="93f69-142">Tjänstevärden</span><span class="sxs-lookup"><span data-stu-id="93f69-142">Service hosting</span></span>
<span data-ttu-id="93f69-143">I Service Fabric-tjänsten körs i en *tjänsten värdprocess*, en körbar fil som körs koden för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="93f69-143">In Service Fabric, your service runs in a *service host process*, an executable file that runs your service code.</span></span> <span data-ttu-id="93f69-144">När du skriver en tjänst med hjälp av tillförlitliga Services API kompilerar bara service-projekt till en körbar fil som registrerar service-typen och kör din kod.</span><span class="sxs-lookup"><span data-stu-id="93f69-144">When you write a service by using the Reliable Services API, your service project just compiles to an executable file that registers your service type and runs your code.</span></span> <span data-ttu-id="93f69-145">Detta gäller i de flesta fall när du skriver en tjänst på Service Fabric i .NET.</span><span class="sxs-lookup"><span data-stu-id="93f69-145">This is true in most cases when you write a service on Service Fabric in .NET.</span></span> <span data-ttu-id="93f69-146">När du öppnar Program.cs i projektet tillståndslösa tjänsten bör du se:</span><span class="sxs-lookup"><span data-stu-id="93f69-146">When you open Program.cs in the stateless service project, you should see:</span></span>

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

<span data-ttu-id="93f69-147">Om du som ser misstänkt ut startpunkten för ett konsolprogram, som är eftersom den är.</span><span class="sxs-lookup"><span data-stu-id="93f69-147">If that looks suspiciously like the entry point to a console application, that's because it is.</span></span>

<span data-ttu-id="93f69-148">Mer information om tjänsten värdprocess och tjänstregistrering ligger utanför omfånget för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="93f69-148">Further details about the service host process and service registration are beyond the scope of this article.</span></span> <span data-ttu-id="93f69-149">Men det är viktigt att veta för nu *service koden körs i sin egen process*.</span><span class="sxs-lookup"><span data-stu-id="93f69-149">But it's important to know for now that *your service code is running in its own process*.</span></span>

## <a name="self-host-web-api-with-an-owin-host"></a><span data-ttu-id="93f69-150">Automatisk värd för webb-API med en OWIN-värd</span><span class="sxs-lookup"><span data-stu-id="93f69-150">Self-host Web API with an OWIN host</span></span>
<span data-ttu-id="93f69-151">Med tanke på att programkoden webb-API finns i en egen process, hur du koppla den till en webbserver?</span><span class="sxs-lookup"><span data-stu-id="93f69-151">Given that your Web API application code is hosted in its own process, how do you hook it up to a web server?</span></span> <span data-ttu-id="93f69-152">Ange [OWIN](http://owin.org/).</span><span class="sxs-lookup"><span data-stu-id="93f69-152">Enter [OWIN](http://owin.org/).</span></span> <span data-ttu-id="93f69-153">OWIN är bara ett avtal mellan .NET-webbprogram och webbservrar.</span><span class="sxs-lookup"><span data-stu-id="93f69-153">OWIN is simply a contract between .NET web applications and web servers.</span></span> <span data-ttu-id="93f69-154">Traditionellt när ASP.NET (upp till MVC 5) används kopplas webbprogrammet nära till IIS via System.Web.</span><span class="sxs-lookup"><span data-stu-id="93f69-154">Traditionally when ASP.NET (up to MVC 5) is used, the web application is tightly coupled to IIS through System.Web.</span></span> <span data-ttu-id="93f69-155">Men implementerar Web API OWIN, så du kan skriva ett program som är fristående från webbservern som värd för den.</span><span class="sxs-lookup"><span data-stu-id="93f69-155">However, Web API implements OWIN, so you can write a web application that is decoupled from the web server that hosts it.</span></span> <span data-ttu-id="93f69-156">Därför kan du använda en *egenvärdbaserat* OWIN-webbserver som du kan starta i en egen process.</span><span class="sxs-lookup"><span data-stu-id="93f69-156">Because of this, you can use a *self-hosted* OWIN web server that you can start in your own process.</span></span> <span data-ttu-id="93f69-157">Detta passar perfekt med Service Fabric-värdmodell vi bara beskrivs.</span><span class="sxs-lookup"><span data-stu-id="93f69-157">This fits perfectly with the Service Fabric hosting model we just described.</span></span>

<span data-ttu-id="93f69-158">I den här artikeln använder vi Katana som värd för OWIN för Web API-program.</span><span class="sxs-lookup"><span data-stu-id="93f69-158">In this article, we'll use Katana as the OWIN host for the Web API application.</span></span> <span data-ttu-id="93f69-159">Katana är en öppen källkod OWIN värd-implementering som bygger på [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) och Windows [http-Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="93f69-159">Katana is an open-source OWIN host implementation built on [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) and the Windows [HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="93f69-160">Mer information om Katana går du till den [Katana plats](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana).</span><span class="sxs-lookup"><span data-stu-id="93f69-160">To learn more about Katana, go to the [Katana site](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana).</span></span> <span data-ttu-id="93f69-161">En snabb överblick över hur du använder Katana själva värd för webb-API finns [använder OWIN till Self-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).</span><span class="sxs-lookup"><span data-stu-id="93f69-161">For a quick overview of how to use Katana to self-host Web API, see [Use OWIN to Self-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).</span></span>
> 
> 

## <a name="set-up-the-web-server"></a><span data-ttu-id="93f69-162">Konfigurera webbservern</span><span class="sxs-lookup"><span data-stu-id="93f69-162">Set up the web server</span></span>
<span data-ttu-id="93f69-163">Reliable Services API ger en startpunkt för kommunikation där du kan ansluta i kommunikation staplar som gör att användare och klienter kan ansluta till tjänsten:</span><span class="sxs-lookup"><span data-stu-id="93f69-163">The Reliable Services API provides a communication entry point where you can plug in communication stacks that allow users and clients to connect to the service:</span></span>

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

<span data-ttu-id="93f69-164">Webbservern (och eventuella andra kommunikation stack du använder i framtiden, till exempel WebSockets) ska använda gränssnittet ICommunicationListener för att integrera korrekt med systemet.</span><span class="sxs-lookup"><span data-stu-id="93f69-164">The web server (and any other communication stack you use in the future, such as WebSockets) should use the ICommunicationListener interface to integrate correctly with the system.</span></span> <span data-ttu-id="93f69-165">Orsaker till detta kommer att bli tydligare i följande steg.</span><span class="sxs-lookup"><span data-stu-id="93f69-165">The reasons for this will become more apparent in the following steps.</span></span>

<span data-ttu-id="93f69-166">Först skapar du en klass med namnet OwinCommunicationListener som implementerar ICommunicationListener:</span><span class="sxs-lookup"><span data-stu-id="93f69-166">First, create a class called OwinCommunicationListener that implements ICommunicationListener:</span></span>

<span data-ttu-id="93f69-167">**OwinCommunicationListener.cs**</span><span class="sxs-lookup"><span data-stu-id="93f69-167">**OwinCommunicationListener.cs**</span></span>

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

<span data-ttu-id="93f69-168">Gränssnittet ICommunicationListener omfattar tre metoder för att hantera en lyssnare för kommunikation för din tjänst:</span><span class="sxs-lookup"><span data-stu-id="93f69-168">The ICommunicationListener interface provides three methods to manage a communication listener for your service:</span></span>

* <span data-ttu-id="93f69-169">*OpenAsync*.</span><span class="sxs-lookup"><span data-stu-id="93f69-169">*OpenAsync*.</span></span> <span data-ttu-id="93f69-170">Börja lyssna efter begäranden.</span><span class="sxs-lookup"><span data-stu-id="93f69-170">Start listening for requests.</span></span>
* <span data-ttu-id="93f69-171">*CloseAsync*.</span><span class="sxs-lookup"><span data-stu-id="93f69-171">*CloseAsync*.</span></span> <span data-ttu-id="93f69-172">Slutar att lyssna efter begäranden, Slutför alla pågående begäranden och avslutas på vanligt sätt.</span><span class="sxs-lookup"><span data-stu-id="93f69-172">Stop listening for requests, finish any in-flight requests, and shut down gracefully.</span></span>
* <span data-ttu-id="93f69-173">*Avbryt*.</span><span class="sxs-lookup"><span data-stu-id="93f69-173">*Abort*.</span></span> <span data-ttu-id="93f69-174">Avbryt allt och stoppa omedelbart.</span><span class="sxs-lookup"><span data-stu-id="93f69-174">Cancel everything and stop immediately.</span></span>

<span data-ttu-id="93f69-175">Kom igång genom att lägga till privata klassmedlemmar för lyssnaren behöver för att fungera.</span><span class="sxs-lookup"><span data-stu-id="93f69-175">To get started, add private class members for things the listener will need to function.</span></span> <span data-ttu-id="93f69-176">Dessa initieras via konstruktorn och användas senare när du ställer in lyssnande URL: en.</span><span class="sxs-lookup"><span data-stu-id="93f69-176">These will be initialized through the constructor and used later when you set up the listening URL.</span></span>

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

## <a name="implement-openasync"></a><span data-ttu-id="93f69-177">Implementera OpenAsync</span><span class="sxs-lookup"><span data-stu-id="93f69-177">Implement OpenAsync</span></span>
<span data-ttu-id="93f69-178">Om du vill konfigurera webbservern, behöver du två typer av information:</span><span class="sxs-lookup"><span data-stu-id="93f69-178">To set up the web server, you need two pieces of information:</span></span>

* <span data-ttu-id="93f69-179">*Ett URL-prefix för sökvägen*.</span><span class="sxs-lookup"><span data-stu-id="93f69-179">*A URL path prefix*.</span></span> <span data-ttu-id="93f69-180">Det här är valfritt, är men bra att konfigurera detta nu så att du kan på ett säkert sätt vara värd för flera webbtjänster i ditt program.</span><span class="sxs-lookup"><span data-stu-id="93f69-180">Although this is optional, it's good for you to set this up now so that you can safely host multiple web services in your application.</span></span>
* <span data-ttu-id="93f69-181">*En port*.</span><span class="sxs-lookup"><span data-stu-id="93f69-181">*A port*.</span></span>

<span data-ttu-id="93f69-182">Det är viktigt att du förstår att Service Fabric ger ett lager för program som fungerar som en buffert mellan programmet och det underliggande operativsystemet som körs på innan du får en port på webbservern.</span><span class="sxs-lookup"><span data-stu-id="93f69-182">Before you get a port for the web server, it's important that you understand that Service Fabric provides an application layer that acts as a buffer between your application and the underlying operating system that it runs on.</span></span> <span data-ttu-id="93f69-183">Därför Service Fabric är ett sätt att konfigurera *slutpunkter* för dina tjänster.</span><span class="sxs-lookup"><span data-stu-id="93f69-183">As such, Service Fabric provides a way to configure *endpoints* for your services.</span></span> <span data-ttu-id="93f69-184">Service Fabric säkerställer att slutpunkter är tillgängliga att använda tjänsten.</span><span class="sxs-lookup"><span data-stu-id="93f69-184">Service Fabric ensures that endpoints are available for your service to use.</span></span> <span data-ttu-id="93f69-185">På så sätt kan du inte behöver konfigurera dem själv i den underliggande OS-miljön.</span><span class="sxs-lookup"><span data-stu-id="93f69-185">This way, you don't have to configure them yourself in the underlying OS environment.</span></span> <span data-ttu-id="93f69-186">Du kan enkelt värd Service Fabric programmet i olika miljöer utan att behöva göra ändringar i ditt program.</span><span class="sxs-lookup"><span data-stu-id="93f69-186">You can easily host your Service Fabric application in different environments without having to make any changes to your application.</span></span> <span data-ttu-id="93f69-187">(Du kan till exempel värd samma program i Azure eller i ditt eget datacenter.)</span><span class="sxs-lookup"><span data-stu-id="93f69-187">(For example, you can host the same application in Azure or in your own datacenter.)</span></span>

<span data-ttu-id="93f69-188">Konfigurera en HTTP-slutpunkt i PackageRoot\ServiceManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="93f69-188">Configure an HTTP endpoint in PackageRoot\ServiceManifest.xml:</span></span>

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

<span data-ttu-id="93f69-189">Det här steget är viktigt eftersom värdprocessen för service körs under autentiseringsuppgifterna för begränsade (Network Service i Windows).</span><span class="sxs-lookup"><span data-stu-id="93f69-189">This step is important because the service host process runs under restricted credentials (Network Service on Windows).</span></span> <span data-ttu-id="93f69-190">Det innebär att tjänsten inte har åtkomst till ställa in en HTTP-slutpunkt på sin egen.</span><span class="sxs-lookup"><span data-stu-id="93f69-190">This means that your service won't have access to set up an HTTP endpoint on its own.</span></span> <span data-ttu-id="93f69-191">Med hjälp av slutpunktskonfigurationen vet Service Fabric för att ställa in rätt åtkomstkontrollistan (ACL) för den URL som tjänsten lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="93f69-191">By using the endpoint configuration, Service Fabric knows to set up the proper access control list (ACL) for the URL that the service will listen on.</span></span> <span data-ttu-id="93f69-192">Service Fabric innehåller också en standardplats för att konfigurera slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="93f69-192">Service Fabric also provides a standard place to configure endpoints.</span></span>

<span data-ttu-id="93f69-193">Tillbaka i OwinCommunicationListener.cs börjar du implementera OpenAsync.</span><span class="sxs-lookup"><span data-stu-id="93f69-193">Back in OwinCommunicationListener.cs, you can start implementing OpenAsync.</span></span> <span data-ttu-id="93f69-194">Det är där du startar webbservern.</span><span class="sxs-lookup"><span data-stu-id="93f69-194">This is where you start the web server.</span></span> <span data-ttu-id="93f69-195">Först hämta slutpunktsinformation om och skapa den URL som tjänsten lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="93f69-195">First, get the endpoint information and create the URL that the service will listen on.</span></span> <span data-ttu-id="93f69-196">URL: en är olika beroende på om lyssnaren används i en tillståndslös tjänst eller en tillståndskänslig service.</span><span class="sxs-lookup"><span data-stu-id="93f69-196">The URL will be different depending on whether the listener is used in a stateless service or a stateful service.</span></span> <span data-ttu-id="93f69-197">För en tillståndskänslig service måste lyssnaren skapa en unik adress för varje tillståndskänslig service replik som den lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="93f69-197">For a stateful service, the listener needs to create a unique address for every stateful service replica it listens on.</span></span> <span data-ttu-id="93f69-198">Adressen kan vara mycket enklare för tillståndslösa tjänster.</span><span class="sxs-lookup"><span data-stu-id="93f69-198">For stateless services, the address can be much simpler.</span></span> 

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

<span data-ttu-id="93f69-199">Observera att ”http://+” användas här.</span><span class="sxs-lookup"><span data-stu-id="93f69-199">Note that "http://+" is used here.</span></span> <span data-ttu-id="93f69-200">Detta är att se till att servern lyssnar på alla tillgängliga adresser, inklusive localhost, FQDN och IP-Adressen för datorn.</span><span class="sxs-lookup"><span data-stu-id="93f69-200">This is to make sure that the web server is listening on all available addresses, including localhost, FQDN, and the machine IP.</span></span>

<span data-ttu-id="93f69-201">OpenAsync-implementering är ett av de viktigaste skälen varför webbservern (eller någon kommunikation stack) implementeras som en ICommunicationListener, istället för bara den öppna direkt från `RunAsync()` i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="93f69-201">The OpenAsync implementation is one of the most important reasons why the web server (or any communication stack) is implemented as an ICommunicationListener, rather than just having it open directly from `RunAsync()` in the service.</span></span> <span data-ttu-id="93f69-202">Returvärdet från OpenAsync är den adress som lyssnar på webbservern.</span><span class="sxs-lookup"><span data-stu-id="93f69-202">The return value from OpenAsync is the address that the web server is listening on.</span></span> <span data-ttu-id="93f69-203">När den här adressen returneras till systemet, registrerar adressen med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="93f69-203">When this address is returned to the system, it registers the address with the service.</span></span> <span data-ttu-id="93f69-204">Service Fabric tillhandahåller ett API som gör att klienter och andra tjänster kan sedan ber för den här adressen av tjänstnamn.</span><span class="sxs-lookup"><span data-stu-id="93f69-204">Service Fabric provides an API that allows clients and other services to then ask for this address by service name.</span></span> <span data-ttu-id="93f69-205">Detta är viktigt eftersom serviceadressen inte är statisk.</span><span class="sxs-lookup"><span data-stu-id="93f69-205">This is important because the service address is not static.</span></span> <span data-ttu-id="93f69-206">Tjänster flyttas i klustret för resursen belastningsutjämning och tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="93f69-206">Services are moved around in the cluster for resource balancing and availability purposes.</span></span> <span data-ttu-id="93f69-207">Det här är den mekanism som gör att klienter kan matcha lyssnande adressen för en tjänst.</span><span class="sxs-lookup"><span data-stu-id="93f69-207">This is the mechanism that allows clients to resolve the listening address for a service.</span></span>

<span data-ttu-id="93f69-208">Med detta i åtanke OpenAsync startar webbservern och returnerar adressen som lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="93f69-208">With that in mind, OpenAsync starts the web server and returns the address that it's listening on.</span></span> <span data-ttu-id="93f69-209">Observera att den lyssnar på ”http://+”, men innan OpenAsync Returnerar adressen på ”+” ersätts med IP- eller FQDN för den används för närvarande på noden.</span><span class="sxs-lookup"><span data-stu-id="93f69-209">Note that it listens on "http://+", but before OpenAsync returns the address, the "+" is replaced with the IP or FQDN of the node it is currently on.</span></span> <span data-ttu-id="93f69-210">Adressen som returneras av den här metoden är vad som har registrerats med systemet.</span><span class="sxs-lookup"><span data-stu-id="93f69-210">The address that is returned by this method is what's registered with the system.</span></span> <span data-ttu-id="93f69-211">Det är också klienter och andra tjänster ser när de frågar efter en tjänst-adress.</span><span class="sxs-lookup"><span data-stu-id="93f69-211">It's also what clients and other services see when they ask for a service's address.</span></span> <span data-ttu-id="93f69-212">För klienter att ansluta korrekt till den, behöver de en faktiska IP eller FQDN i adressen.</span><span class="sxs-lookup"><span data-stu-id="93f69-212">For clients to correctly connect to it, they need an actual IP or FQDN in the address.</span></span>

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

<span data-ttu-id="93f69-213">Observera att detta refererar till Start-klassen som skickades till OwinCommunicationListener i konstruktorn.</span><span class="sxs-lookup"><span data-stu-id="93f69-213">Note that this references the Startup class that was passed in to the OwinCommunicationListener in the constructor.</span></span> <span data-ttu-id="93f69-214">Den här Start-instansen används av webbservern för att starta Web API-program.</span><span class="sxs-lookup"><span data-stu-id="93f69-214">This startup instance is used by the web server to bootstrap the Web API application.</span></span>

<span data-ttu-id="93f69-215">Den `ServiceEventSource.Current.Message()` rad visas i fönstret diagnostiska händelser senare, när du kör programmet för att bekräfta att servern har startats.</span><span class="sxs-lookup"><span data-stu-id="93f69-215">The `ServiceEventSource.Current.Message()` line will appear in the Diagnostic Events window later, when you run the application to confirm that the web server has started successfully.</span></span>

## <a name="implement-closeasync-and-abort"></a><span data-ttu-id="93f69-216">Implementera CloseAsync och ITransaction::Abort</span><span class="sxs-lookup"><span data-stu-id="93f69-216">Implement CloseAsync and Abort</span></span>
<span data-ttu-id="93f69-217">Slutligen implementera både CloseAsync och Avbryt om du vill stoppa webbservern.</span><span class="sxs-lookup"><span data-stu-id="93f69-217">Finally, implement both CloseAsync and Abort to stop the web server.</span></span> <span data-ttu-id="93f69-218">Webbservern kan stoppas av avyttring server-referensen som skapades under OpenAsync.</span><span class="sxs-lookup"><span data-stu-id="93f69-218">The web server can be stopped by disposing the server handle that was created during OpenAsync.</span></span>

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

<span data-ttu-id="93f69-219">I det här implementeringsexemplet stoppa både CloseAsync och Avbryt bara webbservern.</span><span class="sxs-lookup"><span data-stu-id="93f69-219">In this implementation example, both CloseAsync and Abort simply stop the web server.</span></span> <span data-ttu-id="93f69-220">Du kan välja för att utföra en mer korrekt samordnade avstängning för webbservern i CloseAsync.</span><span class="sxs-lookup"><span data-stu-id="93f69-220">You may opt to perform a more gracefully coordinated shutdown of the web server in CloseAsync.</span></span> <span data-ttu-id="93f69-221">Avstängningen kan vänta pågående begäranden ska slutföras innan tillbaka.</span><span class="sxs-lookup"><span data-stu-id="93f69-221">For example, the shutdown could wait for in-flight requests to be completed before the return.</span></span>

## <a name="start-the-web-server"></a><span data-ttu-id="93f69-222">Starta webbservern</span><span class="sxs-lookup"><span data-stu-id="93f69-222">Start the web server</span></span>
<span data-ttu-id="93f69-223">Du är nu redo att skapa och returnera en instans av OwinCommunicationListener att starta webbservern.</span><span class="sxs-lookup"><span data-stu-id="93f69-223">You're now ready to create and return an instance of OwinCommunicationListener to start the web server.</span></span> <span data-ttu-id="93f69-224">Tillbaka i klassen Service (WebService.cs) åsidosätta den `CreateServiceInstanceListeners()` metoden:</span><span class="sxs-lookup"><span data-stu-id="93f69-224">Back in the Service class (WebService.cs), override the `CreateServiceInstanceListeners()` method:</span></span>

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

<span data-ttu-id="93f69-225">Det är där webb-API *programmet* och OWIN *värden* slutligen uppfyller.</span><span class="sxs-lookup"><span data-stu-id="93f69-225">This is where the Web API *application* and the OWIN *host* finally meet.</span></span> <span data-ttu-id="93f69-226">Värden (OwinCommunicationListener) ges en instans av den *programmet* (webb-API) via Start-klassen.</span><span class="sxs-lookup"><span data-stu-id="93f69-226">The host (OwinCommunicationListener) is given an instance of the *application* (Web API) via the Startup class.</span></span> <span data-ttu-id="93f69-227">Service Fabric hanterar sedan dess livscykel.</span><span class="sxs-lookup"><span data-stu-id="93f69-227">Service Fabric then manages its lifecycle.</span></span> <span data-ttu-id="93f69-228">Den här samma mönster kan vanligtvis följas med någon kommunikation stack.</span><span class="sxs-lookup"><span data-stu-id="93f69-228">This same pattern can typically be followed with any communication stack.</span></span>

## <a name="put-it-all-together"></a><span data-ttu-id="93f69-229">Färdigställa allt</span><span class="sxs-lookup"><span data-stu-id="93f69-229">Put it all together</span></span>
<span data-ttu-id="93f69-230">I det här exemplet behöver du inte göra något den `RunAsync()` metod, så att åsidosättningen kan bara tas bort.</span><span class="sxs-lookup"><span data-stu-id="93f69-230">In this example, you don't need to do anything in the `RunAsync()` method, so that override can simply be removed.</span></span>

<span data-ttu-id="93f69-231">Den slutliga implementeringen ska vara mycket enkelt.</span><span class="sxs-lookup"><span data-stu-id="93f69-231">The final service implementation should be very simple.</span></span> <span data-ttu-id="93f69-232">Den behöver bara skapa lyssnaren kommunikation:</span><span class="sxs-lookup"><span data-stu-id="93f69-232">It only needs to create the communication listener:</span></span>

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

<span data-ttu-id="93f69-233">Det fullständiga `OwinCommunicationListener` klass:</span><span class="sxs-lookup"><span data-stu-id="93f69-233">The complete `OwinCommunicationListener` class:</span></span>

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

<span data-ttu-id="93f69-234">Nu när du har placerat alla delar på plats, ditt projekt bör se ut som ett typiskt Web API-program med tillförlitlig Services API startpunkter och en OWIN-värd.</span><span class="sxs-lookup"><span data-stu-id="93f69-234">Now that you have put all the pieces in place, your project should look like a typical Web API application with Reliable Services API entry points and an OWIN host.</span></span>

## <a name="run-and-connect-through-a-web-browser"></a><span data-ttu-id="93f69-235">Kör och ansluter via en webbläsare</span><span class="sxs-lookup"><span data-stu-id="93f69-235">Run and connect through a web browser</span></span>
<span data-ttu-id="93f69-236">Om du inte gjort det, [ställa in din utvecklingsmiljö](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="93f69-236">If you haven't done so, [set up your development environment](service-fabric-get-started.md).</span></span>

<span data-ttu-id="93f69-237">Du kan nu skapa och distribuera din tjänst.</span><span class="sxs-lookup"><span data-stu-id="93f69-237">You can now build and deploy your service.</span></span> <span data-ttu-id="93f69-238">Tryck på **F5** i Visual Studio för att skapa och distribuera programmet.</span><span class="sxs-lookup"><span data-stu-id="93f69-238">Press **F5** in Visual Studio to build and deploy the application.</span></span> <span data-ttu-id="93f69-239">I fönstret diagnostiska händelser bör du se ett meddelande som anger att webbservern öppnas på http://localhost:8281 /.</span><span class="sxs-lookup"><span data-stu-id="93f69-239">In the Diagnostic Events window, you should see a message that indicates that the web server opened on http://localhost:8281/.</span></span>

> [!NOTE]
> <span data-ttu-id="93f69-240">Om porten har redan öppnats av någon annan process på datorn, visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="93f69-240">If the port has already been opened by another process on your machine, you may see an error here.</span></span> <span data-ttu-id="93f69-241">Detta anger att lyssnaren inte kunde öppnas.</span><span class="sxs-lookup"><span data-stu-id="93f69-241">This indicates that the listener couldn't be opened.</span></span> <span data-ttu-id="93f69-242">Om så är fallet kan du prova att använda en annan port för slutpunktskonfigurationen i ServiceManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="93f69-242">If that's the case, try using a different port for the endpoint configuration in ServiceManifest.xml.</span></span>
> 
> 

<span data-ttu-id="93f69-243">När tjänsten körs kan öppna en webbläsare och gå till [http://localhost:8281-api-värden](http://localhost:8281/api/values) att testa den.</span><span class="sxs-lookup"><span data-stu-id="93f69-243">Once the service is running, open a browser and navigate to [http://localhost:8281/api/values](http://localhost:8281/api/values) to test it.</span></span>

## <a name="scale-it-out"></a><span data-ttu-id="93f69-244">Skala ut</span><span class="sxs-lookup"><span data-stu-id="93f69-244">Scale it out</span></span>
<span data-ttu-id="93f69-245">Skala ut tillståndslös webbprogram vanligtvis innebär att lägga till flera datorer och igång webbprogram på dem..</span><span class="sxs-lookup"><span data-stu-id="93f69-245">Scaling out stateless web apps typically means adding more machines and spinning up the web apps on them.</span></span> <span data-ttu-id="93f69-246">Service Fabric orchestration-motorn kan göra när nya noder läggs till ett kluster.</span><span class="sxs-lookup"><span data-stu-id="93f69-246">Service Fabric's orchestration engine can do this for you whenever new nodes are added to a cluster.</span></span> <span data-ttu-id="93f69-247">När du skapar instanser av tillståndslösa tjänsten måste ange du antalet instanser som du vill skapa.</span><span class="sxs-lookup"><span data-stu-id="93f69-247">When you create instances of a stateless service, you can specify the number of instances you want to create.</span></span> <span data-ttu-id="93f69-248">Service Fabric placeras det antalet instanser på noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="93f69-248">Service Fabric places that number of instances on nodes in the cluster.</span></span> <span data-ttu-id="93f69-249">Och det gör att inte skapa fler än en instans på varje nod i ett.</span><span class="sxs-lookup"><span data-stu-id="93f69-249">And it makes sure not to create more than one instance on any one node.</span></span> <span data-ttu-id="93f69-250">Du kan också instruera Service Fabric du alltid vill skapa en instans på varje nod genom att ange **-1** för instansantal.</span><span class="sxs-lookup"><span data-stu-id="93f69-250">You can also instruct Service Fabric to always create an instance on every node by specifying **-1** for the instance count.</span></span> <span data-ttu-id="93f69-251">Detta garanterar att när du lägger till noder för att skala ut ditt kluster en instans av tillståndslösa tjänsten kommer att skapas på de nya noderna.</span><span class="sxs-lookup"><span data-stu-id="93f69-251">This guarantees that whenever you add nodes to scale out your cluster, an instance of your stateless service will be created on the new nodes.</span></span> <span data-ttu-id="93f69-252">Det här värdet är en egenskap för tjänstinstansen, så anges när du skapar en instans av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="93f69-252">This value is a property of the service instance, so it is set when you create a service instance.</span></span> <span data-ttu-id="93f69-253">Du kan göra detta med hjälp av PowerShell:</span><span class="sxs-lookup"><span data-stu-id="93f69-253">You can do this through PowerShell:</span></span>

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

<span data-ttu-id="93f69-254">Du kan också göra detta när du definierar en standardtjänst i ett Visual Studio-projekt för tillståndslösa tjänsten:</span><span class="sxs-lookup"><span data-stu-id="93f69-254">You can also do this when you define a default service in a Visual Studio stateless service project:</span></span>

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

<span data-ttu-id="93f69-255">Mer information om hur du skapar program och instanser av tjänsten finns [distribuera ett program](service-fabric-deploy-remove-applications.md).</span><span class="sxs-lookup"><span data-stu-id="93f69-255">For more information on how to create application and service instances, see [Deploy an application](service-fabric-deploy-remove-applications.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="93f69-256">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="93f69-256">Next steps</span></span>
[<span data-ttu-id="93f69-257">Felsöka Service Fabric-program med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93f69-257">Debug your Service Fabric application by using Visual Studio</span></span>](service-fabric-debugging-your-application.md)

