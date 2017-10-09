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
# <a name="aspnet-core-in-service-fabric-reliable-services"></a><span data-ttu-id="d9563-103">ASP.NET Core i Service Fabric Reliable Services</span><span class="sxs-lookup"><span data-stu-id="d9563-103">ASP.NET Core in Service Fabric Reliable Services</span></span>

<span data-ttu-id="d9563-104">ASP.NET Core är ett nytt öppen källkod och plattformsoberoende ramverk för att bygga moderna molnbaserade Internetanslutna program, till exempel webbappar, IoT-appar och mobila serverdelar.</span><span class="sxs-lookup"><span data-stu-id="d9563-104">ASP.NET Core is a new open-source and cross-platform framework for building modern cloud-based Internet-connected applications, such as web apps, IoT apps, and mobile backends.</span></span> 

<span data-ttu-id="d9563-105">Den här artikeln är en djupgående guide toohosting ASP.NET Core services i Service Fabric Reliable Services med hello **Microsoft.ServiceFabric.AspNetCore.** * uppsättning NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="d9563-105">This article is an in-depth guide toohosting ASP.NET Core services in Service Fabric Reliable Services using hello **Microsoft.ServiceFabric.AspNetCore.*** set of NuGet packages.</span></span>

<span data-ttu-id="d9563-106">En inledande självstudiekurs om ASP.NET Core i Service Fabric och anvisningar om hur din utvecklingsmiljö konfigurera finns i [bygga en webbklientdel för ditt program med hjälp av ASP.NET Core](service-fabric-add-a-web-frontend.md).</span><span class="sxs-lookup"><span data-stu-id="d9563-106">For an introductory tutorial on ASP.NET Core in Service Fabric and instructions on getting your development environment set up, see [Building a web front-end for your application using ASP.NET Core](service-fabric-add-a-web-frontend.md).</span></span>

<span data-ttu-id="d9563-107">hello resten av den här artikeln förutsätter att du redan är bekant med ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d9563-107">hello rest of this article assumes you are already familiar with ASP.NET Core.</span></span> <span data-ttu-id="d9563-108">Om inte, vi rekommenderar att läsa igenom hello [ASP.NET grunderna](https://docs.microsoft.com/aspnet/core/fundamentals/index).</span><span class="sxs-lookup"><span data-stu-id="d9563-108">If not, we recommend reading through hello [ASP.NET Core fundamentals](https://docs.microsoft.com/aspnet/core/fundamentals/index).</span></span>

## <a name="aspnet-core-in-hello-service-fabric-environment"></a><span data-ttu-id="d9563-109">ASP.NET Core i hello Service Fabric-miljö</span><span class="sxs-lookup"><span data-stu-id="d9563-109">ASP.NET Core in hello Service Fabric environment</span></span>

<span data-ttu-id="d9563-110">När ASP.NET Core appar kan köras på .NET Core eller hello fullständig .NET Framework, Service Fabric-tjänster för närvarande kan endast köras på hello fullständig .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d9563-110">While ASP.NET Core apps can run on .NET Core or on hello full .NET Framework, Service Fabric services currently can only run on hello full .NET Framework.</span></span> <span data-ttu-id="d9563-111">Det innebär när du skapar en ASP.NET Core Service Fabric-tjänst, du måste fortfarande använda hello fullständig .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d9563-111">This means when you build an ASP.NET  Core Service Fabric service, you must still target hello full .NET Framework.</span></span>

<span data-ttu-id="d9563-112">ASP.NET Core kan användas på två olika sätt i Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="d9563-112">ASP.NET Core can be used in two different ways in Service Fabric:</span></span>
 - <span data-ttu-id="d9563-113">**Värdbaserad som en gäst körbar fil**.</span><span class="sxs-lookup"><span data-stu-id="d9563-113">**Hosted as a guest executable**.</span></span> <span data-ttu-id="d9563-114">Detta är främst används toorun befintliga ASP.NET Core program på Service Fabric utan ändringar i koden.</span><span class="sxs-lookup"><span data-stu-id="d9563-114">This is primarily used toorun existing ASP.NET Core applications on Service Fabric with no code changes.</span></span>
 - <span data-ttu-id="d9563-115">**Körs i en tillförlitlig tjänst**.</span><span class="sxs-lookup"><span data-stu-id="d9563-115">**Run inside a Reliable Service**.</span></span> <span data-ttu-id="d9563-116">Detta kan bättre integration med hello Service Fabric runtime och tillståndskänsliga ASP.NET Core services.</span><span class="sxs-lookup"><span data-stu-id="d9563-116">This allows better integration with hello Service Fabric runtime and allows stateful ASP.NET Core services.</span></span>

<span data-ttu-id="d9563-117">hello resten av den här artikeln förklarar hur toouse ASP.NET Core inuti en tillförlitlig tjänst med hjälp av hello ASP.NET Core integrationskomponenterna som levereras med hello Service Fabric-SDK.</span><span class="sxs-lookup"><span data-stu-id="d9563-117">hello rest of this article explains how toouse ASP.NET Core inside a Reliable Service using hello ASP.NET Core integration components that ship with hello Service Fabric SDK.</span></span> 

## <a name="service-fabric-service-hosting"></a><span data-ttu-id="d9563-118">Tjänsten värd för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="d9563-118">Service Fabric service hosting</span></span>

<span data-ttu-id="d9563-119">I Service Fabric en eller flera instanser och/eller repliker av din tjänst köras i en *tjänsten värdprocess*, en körbar fil som körs koden för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d9563-119">In Service Fabric, one or more instances and/or replicas of your service run in a *service host process*, an executable file that runs your service code.</span></span> <span data-ttu-id="d9563-120">Du, som en tjänst författare äger hello serverprocess och Service Fabric aktiveras och övervakar du.</span><span class="sxs-lookup"><span data-stu-id="d9563-120">You, as a service author, own hello service host process and Service Fabric activates and monitors it for you.</span></span>

<span data-ttu-id="d9563-121">Traditionella ASP.NET (upp tooMVC 5) är tätt kopplade tooIIS via System.Web.dll.</span><span class="sxs-lookup"><span data-stu-id="d9563-121">Traditional ASP.NET (up tooMVC 5) is tightly coupled tooIIS through System.Web.dll.</span></span> <span data-ttu-id="d9563-122">ASP.NET Core ger en åtskillnad mellan hello webbserver och ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d9563-122">ASP.NET Core provides a separation between hello web server and your web application.</span></span> <span data-ttu-id="d9563-123">Detta gör web applications toobe bärbar mellan olika webbservrar och kan också web servrar toobe *egenvärdbaserat*, vilket innebär att du kan starta en webbserver i en egen process som skillnad från tooa process som ägs av dedicerad serverprogram, till exempel IIS.</span><span class="sxs-lookup"><span data-stu-id="d9563-123">This allows web applications toobe portable between different web servers and also allows web servers toobe *self-hosted*, which means you can start a web server in your own process, as opposed tooa process that is owned by dedicated web server software such as IIS.</span></span> 

<span data-ttu-id="d9563-124">Du måste vara kan toostart ASP.NET i din serverprocess i ordning toocombine Service Fabric-tjänsten och ASP.NET, antingen som en gäst körbar fil eller i en tillförlitlig tjänst.</span><span class="sxs-lookup"><span data-stu-id="d9563-124">In order toocombine a Service Fabric service and ASP.NET, either as a guest executable or in a Reliable Service, you must be able toostart ASP.NET inside your service host process.</span></span> <span data-ttu-id="d9563-125">ASP.NET Core värd själv kan du toodo detta.</span><span class="sxs-lookup"><span data-stu-id="d9563-125">ASP.NET Core self-hosting allows you toodo this.</span></span>

## <a name="hosting-aspnet-core-in-a-reliable-service"></a><span data-ttu-id="d9563-126">Värd för ASP.NET Core i en tillförlitlig tjänst</span><span class="sxs-lookup"><span data-stu-id="d9563-126">Hosting ASP.NET Core in a Reliable Service</span></span>
<span data-ttu-id="d9563-127">Vanligtvis själva ASP.NET Core program skapar ett vid startpunkten för ett program, till exempel hello `static void Main()` metod i `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="d9563-127">Typically, self-hosted ASP.NET Core applications create a WebHost in an application's entry point, such as hello `static void Main()` method in `Program.cs`.</span></span> <span data-ttu-id="d9563-128">I det här fallet är hello livscykeln för hello Webbvärden bundna toohello livscykeln för hello-processen.</span><span class="sxs-lookup"><span data-stu-id="d9563-128">In this case, hello lifecycle of hello WebHost is bound toohello lifecycle of hello process.</span></span>

![Värd för ASP.NET Core i en process][0]

<span data-ttu-id="d9563-130">Dock hello startpunkt för programmet är inte hello rätt plats toocreate en Webbvärden i en tillförlitlig tjänst, eftersom hello startpunkt för programmet används bara tooregister en tjänst-typ med hello Service Fabric runtime, så att den kan skapa instanser av tjänsten Ange.</span><span class="sxs-lookup"><span data-stu-id="d9563-130">However, hello application entry point is not hello right place toocreate a WebHost in a Reliable Service, because hello application entry point is only used tooregister a service type with hello Service Fabric runtime, so that it may create instances of that service type.</span></span> <span data-ttu-id="d9563-131">hello Webbvärden ska skapas i en tillförlitlig tjänst sig själv.</span><span class="sxs-lookup"><span data-stu-id="d9563-131">hello WebHost should be created in a Reliable Service itself.</span></span> <span data-ttu-id="d9563-132">Instanser av tjänsten och/eller repliker kan gå igenom flera livscykler inom värdprocess för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d9563-132">Within hello service host process, service instances and/or replicas can go through multiple lifecycles.</span></span> 

<span data-ttu-id="d9563-133">En tillförlitlig tjänstinstans representeras av din tjänstklass som härleds från `StatelessService` eller `StatefulService`.</span><span class="sxs-lookup"><span data-stu-id="d9563-133">A Reliable Service instance is represented by your service class deriving from `StatelessService` or `StatefulService`.</span></span> <span data-ttu-id="d9563-134">hello kommunikation stack för en tjänst som finns i en `ICommunicationListener` implementering service-klassen.</span><span class="sxs-lookup"><span data-stu-id="d9563-134">hello communication stack for a service is contained in an `ICommunicationListener` implementation in your service class.</span></span> <span data-ttu-id="d9563-135">Hej `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet-paket som innehåller implementeringar av `ICommunicationListener` som starta och hantera hello ASP.NET Core Webbvärden för Kestrel eller WebListener i en tillförlitlig tjänst.</span><span class="sxs-lookup"><span data-stu-id="d9563-135">hello `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet packages contain implementations of `ICommunicationListener` that start and manage hello ASP.NET Core WebHost for either Kestrel or WebListener in a Reliable Service.</span></span>

![Värd för ASP.NET Core i en tillförlitlig tjänst][1]

## <a name="aspnet-core-icommunicationlisteners"></a><span data-ttu-id="d9563-137">ASP.NET Core ICommunicationListeners</span><span class="sxs-lookup"><span data-stu-id="d9563-137">ASP.NET Core ICommunicationListeners</span></span>
<span data-ttu-id="d9563-138">Hej `ICommunicationListener` implementeringar för Kestrel och WebListener i hello `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet-paket har liknande användningsmönster men utföra lite olika åtgärder specifika tooeach webbservern.</span><span class="sxs-lookup"><span data-stu-id="d9563-138">hello `ICommunicationListener` implementations for Kestrel and WebListener in hello  `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet packages have similar use patterns but perform slightly different actions specific tooeach web server.</span></span> 

<span data-ttu-id="d9563-139">Både kommunikationslyssnarna ger en konstruktor som tar hello följande argument:</span><span class="sxs-lookup"><span data-stu-id="d9563-139">Both communication listeners provide a constructor that takes hello following arguments:</span></span>
 - <span data-ttu-id="d9563-140">**`ServiceContext serviceContext`**: hello `ServiceContext` objekt som innehåller information om hello tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="d9563-140">**`ServiceContext serviceContext`**: hello `ServiceContext` object that contains information about hello running service.</span></span>
 - <span data-ttu-id="d9563-141">**`string endpointName`**: hello namnet på en `Endpoint` konfigurationen i ServiceManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="d9563-141">**`string endpointName`**: hello name of an `Endpoint` configuration in ServiceManifest.xml.</span></span> <span data-ttu-id="d9563-142">Detta är främst där hello två kommunikationslyssnarna skiljer sig: WebListener **kräver** en `Endpoint` konfiguration, men inte av Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d9563-142">This is primarily where hello two communication listeners differ: WebListener **requires** an `Endpoint` configuration, while Kestrel does not.</span></span>
 - <span data-ttu-id="d9563-143">**`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: ett lambda-uttryck som du implementerar i som du skapar och returnera ett `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="d9563-143">**`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: a lambda that you implement in which you create and return an `IWebHost`.</span></span> <span data-ttu-id="d9563-144">Detta gör att du tooconfigure `IWebHost` hello sätt som vanligt i ett program för ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d9563-144">This allows you tooconfigure `IWebHost` hello way you normally would in an ASP.NET Core application.</span></span> <span data-ttu-id="d9563-145">hello lambda innehåller en URL som har genererats för du beroende på hello Service Fabric-integrering alternativen du användning och hello `Endpoint` konfiguration som du anger.</span><span class="sxs-lookup"><span data-stu-id="d9563-145">hello lambda provides a URL which is generated for you depending on hello Service Fabric integration options you use and hello `Endpoint` configuration you provide.</span></span> <span data-ttu-id="d9563-146">Att URL: en sedan kan ändras eller används som-är toostart hello-webbserver.</span><span class="sxs-lookup"><span data-stu-id="d9563-146">That URL can then be modified or used as-is toostart hello web server.</span></span>

## <a name="service-fabric-integration-middleware"></a><span data-ttu-id="d9563-147">Mellanprogram för Service Fabric-integrering</span><span class="sxs-lookup"><span data-stu-id="d9563-147">Service Fabric integration middleware</span></span>
<span data-ttu-id="d9563-148">Hej `Microsoft.ServiceFabric.Services.AspNetCore` NuGet-paketet innehåller hello `UseServiceFabricIntegration` tilläggsmetoden på `IWebHostBuilder` som lägger till Service Fabric-medvetna mellanprogram.</span><span class="sxs-lookup"><span data-stu-id="d9563-148">hello `Microsoft.ServiceFabric.Services.AspNetCore` NuGet package includes hello `UseServiceFabricIntegration` extension method on `IWebHostBuilder` that adds Service Fabric-aware middleware.</span></span> <span data-ttu-id="d9563-149">Den här mellanprogram konfigurerar hello Kestrel eller WebListener `ICommunicationListener` tooregister en unika tjänst-URL med hello namngivningstjänst för Service Fabric och sedan validerar klient begäranden tooensure klienter ansluter toohello tjänst.</span><span class="sxs-lookup"><span data-stu-id="d9563-149">This middleware configures hello Kestrel or WebListener `ICommunicationListener` tooregister a unique service URL with hello Service Fabric Naming Service and then validates client requests tooensure clients are connecting toohello right service.</span></span> <span data-ttu-id="d9563-150">Detta är nödvändigt i en miljö med delad värden, till exempel Service Fabric där flera webbprogram kan köras på hello samma fysiska eller virtuella datorn men använder inte unika värdnamn, tooprevent klienter från att av misstag ansluta toohello fel tjänst.</span><span class="sxs-lookup"><span data-stu-id="d9563-150">This is necessary in a shared-host environment such as Service Fabric, where multiple web applications can run on hello same physical or virtual machine but do not use unique host names, tooprevent clients from mistakenly connecting toohello wrong service.</span></span> <span data-ttu-id="d9563-151">Det här scenariot beskrivs i detalj i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d9563-151">This scenario is described in more detail in hello next section.</span></span>

### <a name="a-case-of-mistaken-identity"></a><span data-ttu-id="d9563-152">Ett fall av felaktig identitet</span><span class="sxs-lookup"><span data-stu-id="d9563-152">A case of mistaken identity</span></span>
<span data-ttu-id="d9563-153">Tjänsten repliker, oavsett protokoll, lyssna på en unik IP:port kombination.</span><span class="sxs-lookup"><span data-stu-id="d9563-153">Service replicas, regardless of protocol, listen on a unique IP:port combination.</span></span> <span data-ttu-id="d9563-154">När en replik för tjänsten har börjat lyssna på en IP:port slutpunkt, rapporterar att slutpunkten adress toohello namngivningstjänst för Service Fabric där den kan identifieras av klienter eller andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="d9563-154">Once a service replica has started listening on an IP:port endpoint, it reports that endpoint address toohello Service Fabric Naming Service where it can be discovered by clients or other services.</span></span> <span data-ttu-id="d9563-155">Om tjänster använder dynamiskt tilldelade programmet portar, en tjänst replik tillfälligtvis kan använda hello samma IP:port slutpunkten för en annan tjänst som tidigare fanns på hello samma fysiska eller virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d9563-155">If services use dynamically-assigned application ports, a service replica may coincidentally use hello same IP:port endpoint of another service that was previously on hello same physical or virtual machine.</span></span> <span data-ttu-id="d9563-156">Detta kan medföra att en klient toomistakely ansluta toohello fel tjänst.</span><span class="sxs-lookup"><span data-stu-id="d9563-156">This can cause a client toomistakely connect toohello wrong service.</span></span> <span data-ttu-id="d9563-157">Detta kan inträffa om hello följande händelser inträffar:</span><span class="sxs-lookup"><span data-stu-id="d9563-157">This can happen if hello following sequence of events occur:</span></span>

 1. <span data-ttu-id="d9563-158">Tjänsten A lyssnar på 10.0.0.1:30000 via HTTP.</span><span class="sxs-lookup"><span data-stu-id="d9563-158">Service A listens on 10.0.0.1:30000 over HTTP.</span></span> 
 2. <span data-ttu-id="d9563-159">Klienten matchar A Service och hämtar adress 10.0.0.1:30000</span><span class="sxs-lookup"><span data-stu-id="d9563-159">Client resolves Service A and gets address 10.0.0.1:30000</span></span>
 3. <span data-ttu-id="d9563-160">En annan nod flyttar tooa-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d9563-160">Service A moves tooa different node.</span></span>
 4. <span data-ttu-id="d9563-161">Tjänsten B är placerad 10.0.0.1 och tillfälligtvis hello använder samma port 30000.</span><span class="sxs-lookup"><span data-stu-id="d9563-161">Service B is placed on 10.0.0.1 and coincidentally uses hello same port 30000.</span></span>
 5. <span data-ttu-id="d9563-162">Klienten försöker tooconnect tooservice A med cachelagrade adress 10.0.0.1:30000.</span><span class="sxs-lookup"><span data-stu-id="d9563-162">Client attempts tooconnect tooservice A with cached address 10.0.0.1:30000.</span></span>
 6. <span data-ttu-id="d9563-163">Klienten är nu ansluten tooservice B märker inte den anslutna toohello fel tjänst.</span><span class="sxs-lookup"><span data-stu-id="d9563-163">Client is now successfully connected tooservice B not realizing it is connected toohello wrong service.</span></span>

<span data-ttu-id="d9563-164">Detta kan orsaka fel vid slumpmässiga tidpunkter som kan vara svårt toodiagnose.</span><span class="sxs-lookup"><span data-stu-id="d9563-164">This can cause bugs at random times that can be difficult toodiagnose.</span></span> 

### <a name="using-unique-service-urls"></a><span data-ttu-id="d9563-165">Med unika URL: er</span><span class="sxs-lookup"><span data-stu-id="d9563-165">Using unique service URLs</span></span>
<span data-ttu-id="d9563-166">tooprevent, services efter en slutpunkt toohello Naming Service med en unik identifierare och sedan validera den unika identifieraren under klientbegäranden.</span><span class="sxs-lookup"><span data-stu-id="d9563-166">tooprevent this, services can post an endpoint toohello Naming Service with a unique identifier, and then validate that unique identifier during client requests.</span></span> <span data-ttu-id="d9563-167">Detta är en samordnad åtgärd mellan tjänster i en betrodd miljö för icke-farliga-klient.</span><span class="sxs-lookup"><span data-stu-id="d9563-167">This is a cooperative action between services in a non-hostile-tenant trusted environment.</span></span> <span data-ttu-id="d9563-168">Detta ger inte säkra autentiseringsuppgifter i en miljö med farliga klient.</span><span class="sxs-lookup"><span data-stu-id="d9563-168">This does not provide secure service authentication in a hostile-tenant environment.</span></span>

<span data-ttu-id="d9563-169">I en betrodd miljö hello mellanprogram som läggs till av hello `UseServiceFabricIntegration` metoden lägger automatiskt till en unik identifierare toohello-adress som är bokförd toohello Naming Service och validerar den identifieraren för varje begäran.</span><span class="sxs-lookup"><span data-stu-id="d9563-169">In a trusted environment, hello middleware that's added by hello `UseServiceFabricIntegration` method automatically appends a unique identifier toohello address that is posted toohello Naming Service and validates that identifier on each request.</span></span> <span data-ttu-id="d9563-170">Om hello-ID inte matchar returnerar hello mellanprogram omedelbart ett HTTP 410 rest-svar.</span><span class="sxs-lookup"><span data-stu-id="d9563-170">If hello identifier does not match, hello middleware immediately returns an HTTP 410 Gone response.</span></span>

<span data-ttu-id="d9563-171">Tjänster som använder en dynamiskt tilldelad port bör se användning av den här mellanprogram.</span><span class="sxs-lookup"><span data-stu-id="d9563-171">Services that use a dynamically-assigned port should make use of this middleware.</span></span>

<span data-ttu-id="d9563-172">Tjänster som använder en fast unika port har inte det här problemet i en integrerad miljö.</span><span class="sxs-lookup"><span data-stu-id="d9563-172">Services that use a fixed unique port do not have this problem in a cooperative environment.</span></span> <span data-ttu-id="d9563-173">En fast unika port används vanligtvis för externt riktade som behöver en välkända port för klienten program tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="d9563-173">A fixed unique port is typically used for externally-facing services that need a well-known port for client applications tooconnect to.</span></span> <span data-ttu-id="d9563-174">Till exempel använder de flesta Internet-riktade webbprogram port 80 eller 443 för web webbläsare anslutningar.</span><span class="sxs-lookup"><span data-stu-id="d9563-174">For example, most Internet-facing web applications will use port 80 or 443 for web browser connections.</span></span> <span data-ttu-id="d9563-175">I det här fallet ska hello Unik identifierare inte aktiveras.</span><span class="sxs-lookup"><span data-stu-id="d9563-175">In this case, hello unique identifier should not be enabled.</span></span>

<span data-ttu-id="d9563-176">hello följande diagram visar hello begäran flödar med hello mellanprogram aktiverad:</span><span class="sxs-lookup"><span data-stu-id="d9563-176">hello following diagram shows hello request flow with hello middleware enabled:</span></span>

![Service Fabric ASP.NET Core-integrering][2]

<span data-ttu-id="d9563-178">Både Kestrel och WebListener `ICommunicationListener` implementeringar använder den här mekanismen i hello exakt samma sätt.</span><span class="sxs-lookup"><span data-stu-id="d9563-178">Both Kestrel and WebListener `ICommunicationListener` implementations use this mechanism in exactly hello same way.</span></span> <span data-ttu-id="d9563-179">Även om WebListener internt kan skilja förfrågningar baserat på unika URL-sökvägar med hjälp av hello underliggande *http.sys* portdelning funktion som funktionen är *inte* används av hello WebListener `ICommunicationListener` implementering eftersom som leder HTTP 503- och HTTP 404-fel statuskoder i hello-scenario som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="d9563-179">Although WebListener can internally differentiate requests based on unique URL paths using hello underlying *http.sys* port sharing feature, that functionality is *not* used by hello WebListener `ICommunicationListener` implementation because that will result in HTTP 503 and HTTP 404 error status codes in hello scenario described earlier.</span></span> <span data-ttu-id="d9563-180">Som gör i sin tur det mycket svårt för klienter toodetermine hello syftet med hello felet HTTP 503- och HTTP 404 är redan används ofta tooindicate andra fel.</span><span class="sxs-lookup"><span data-stu-id="d9563-180">That in turn makes it very difficult for clients toodetermine hello intent of hello error, as HTTP 503 and HTTP 404 are already commonly used tooindicate other errors.</span></span> <span data-ttu-id="d9563-181">Därför både Kestrel och WebListener `ICommunicationListener` implementeringar standardisera hello mellanprogram som tillhandahålls av hello `UseServiceFabricIntegration` tilläggsmetoden så att klienter behöver bara tooperform en tjänstslutpunkt nytt lösa åtgärd på 410 HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="d9563-181">Thus, both Kestrel and WebListener `ICommunicationListener` implementations standardize on hello middleware provided by hello `UseServiceFabricIntegration` extension method so that clients only need tooperform a service endpoint re-resolve action on HTTP 410 responses.</span></span>

## <a name="weblistener-in-reliable-services"></a><span data-ttu-id="d9563-182">WebListener i Reliable Services</span><span class="sxs-lookup"><span data-stu-id="d9563-182">WebListener in Reliable Services</span></span>
<span data-ttu-id="d9563-183">WebListener kan användas i en tillförlitlig tjänst genom att importera hello **Microsoft.ServiceFabric.AspNetCore.WebListener** NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="d9563-183">WebListener can be used in a Reliable Service by importing hello **Microsoft.ServiceFabric.AspNetCore.WebListener** NuGet package.</span></span> <span data-ttu-id="d9563-184">Det här paketet innehåller `WebListenerCommunicationListener`, en implementering av `ICommunicationListener`, som gör att du toocreate en ASP.NET Core Webbvärden inuti en tillförlitlig tjänst med WebListener som hello webbserver.</span><span class="sxs-lookup"><span data-stu-id="d9563-184">This package contains `WebListenerCommunicationListener`, an implementation of `ICommunicationListener`, that allows you toocreate an ASP.NET Core WebHost inside a Reliable Service using WebListener as hello web server.</span></span>

<span data-ttu-id="d9563-185">WebListener bygger på hello [Windows http-Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="d9563-185">WebListener is built on hello [Windows HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx).</span></span> <span data-ttu-id="d9563-186">Här används hello *http.sys* kernel-drivrutinen används av IIS tooprocess HTTP-begäranden och vidarebefordra dem tooprocesses webbprogram som körs.</span><span class="sxs-lookup"><span data-stu-id="d9563-186">This uses hello *http.sys* kernel driver used by IIS tooprocess HTTP requests and route them tooprocesses running web applications.</span></span> <span data-ttu-id="d9563-187">Detta gör flera processer på hello samma fysiska eller virtuella datorn toohost webbprogram på hello samma port, skiljas åt av en unik URL-sökvägen eller värdnamn.</span><span class="sxs-lookup"><span data-stu-id="d9563-187">This allows multiple processes on hello same physical or virtual machine toohost web applications on hello same port, disambiguated by either a unique URL path or hostname.</span></span> <span data-ttu-id="d9563-188">Dessa funktioner är användbara i Service Fabric som värd för flera webbplatser i hello samma kluster.</span><span class="sxs-lookup"><span data-stu-id="d9563-188">These features are useful in Service Fabric for hosting multiple websites in hello same cluster.</span></span>

<span data-ttu-id="d9563-189">hello följande diagram illustrerar hur WebListener använder hello *http.sys* kernel-drivrutinen i Windows för delning av port:</span><span class="sxs-lookup"><span data-stu-id="d9563-189">hello following diagram illustrates how WebListener uses hello *http.sys* kernel driver on Windows for port sharing:</span></span>

![HTTP.sys][3]

### <a name="weblistener-in-a-stateless-service"></a><span data-ttu-id="d9563-191">WebListener i en tillståndslös tjänst</span><span class="sxs-lookup"><span data-stu-id="d9563-191">WebListener in a stateless service</span></span>
<span data-ttu-id="d9563-192">toouse `WebListener` i en tillståndslös tjänst åsidosätta hello `CreateServiceInstanceListeners` metod och returnera ett `WebListenerCommunicationListener` instans:</span><span class="sxs-lookup"><span data-stu-id="d9563-192">toouse `WebListener` in a stateless service, override hello `CreateServiceInstanceListeners` method and return a `WebListenerCommunicationListener` instance:</span></span>

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

### <a name="weblistener-in-a-stateful-service"></a><span data-ttu-id="d9563-193">WebListener i en tillståndskänslig service</span><span class="sxs-lookup"><span data-stu-id="d9563-193">WebListener in a stateful service</span></span>

<span data-ttu-id="d9563-194">`WebListenerCommunicationListener`för närvarande är inte avsedd för användning i tillståndskänsliga tjänster på grund av toocomplications med hello underliggande *http.sys* portdelning funktionen.</span><span class="sxs-lookup"><span data-stu-id="d9563-194">`WebListenerCommunicationListener` is currently not designed for use in stateful services due toocomplications with hello underlying *http.sys* port sharing feature.</span></span> <span data-ttu-id="d9563-195">Mer information finns i följande avsnitt om dynamisk porttilldelning med WebListener hello.</span><span class="sxs-lookup"><span data-stu-id="d9563-195">For more information, see hello following section on dynamic port allocation with WebListener.</span></span> <span data-ttu-id="d9563-196">För tillståndskänsliga tjänster är Kestrel hello rekommenderas webbservern.</span><span class="sxs-lookup"><span data-stu-id="d9563-196">For stateful services, Kestrel is hello recommended web server.</span></span>

### <a name="endpoint-configuration"></a><span data-ttu-id="d9563-197">Slutpunktskonfiguration</span><span class="sxs-lookup"><span data-stu-id="d9563-197">Endpoint configuration</span></span>

<span data-ttu-id="d9563-198">En `Endpoint` konfiguration krävs för webbservrar som använder hello API Windows HTTP-servern, inklusive WebListener.</span><span class="sxs-lookup"><span data-stu-id="d9563-198">An `Endpoint` configuration is required for web servers that use hello Windows HTTP Server API, including WebListener.</span></span> <span data-ttu-id="d9563-199">Webbservrar som använder hello Windows API för HTTP-servern måste först reservera URL-Adressen med *http.sys* (detta görs normalt med hello [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) tool).</span><span class="sxs-lookup"><span data-stu-id="d9563-199">Web servers that use hello Windows HTTP Server API must first reserve their URL with *http.sys* (this is normally accomplished with hello [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) tool).</span></span> <span data-ttu-id="d9563-200">Den här åtgärden kräver förhöjd behörighet som inte har dina tjänster som standard.</span><span class="sxs-lookup"><span data-stu-id="d9563-200">This action requires elevated privileges that your services by default do not have.</span></span> <span data-ttu-id="d9563-201">Hej ”http” eller ”https” alternativ för hello `Protocol` -egenskapen för hello `Endpoint` konfigurationen i *ServiceManifest.xml* används specifikt tooinstruct hello Service Fabric runtime tooregister en URL med *http.sys* för din räkning som använder hello [ *starkt jokertecken* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL-prefix.</span><span class="sxs-lookup"><span data-stu-id="d9563-201">hello "http" or "https" options for hello `Protocol` property of hello `Endpoint` configuration in *ServiceManifest.xml* are used specifically tooinstruct hello Service Fabric runtime tooregister a URL with *http.sys* on your behalf using hello [*strong wildcard*](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL prefix.</span></span>

<span data-ttu-id="d9563-202">Till exempel tooreserve `http://+:80` för en tjänst hello följande konfiguration ska användas i ServiceManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="d9563-202">For example, tooreserve `http://+:80` for a service, hello following configuration should be used in ServiceManifest.xml:</span></span>

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

<span data-ttu-id="d9563-203">Och namnet på slutpunkten hello måste överföras toohello `WebListenerCommunicationListener` konstruktorn:</span><span class="sxs-lookup"><span data-stu-id="d9563-203">And hello endpoint name must be passed toohello `WebListenerCommunicationListener` constructor:</span></span>

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

#### <a name="use-weblistener-with-a-static-port"></a><span data-ttu-id="d9563-204">Använda WebListener med en statisk port</span><span class="sxs-lookup"><span data-stu-id="d9563-204">Use WebListener with a static port</span></span>
<span data-ttu-id="d9563-205">toouse en statisk port med WebListener, ange hello portnummer i hello `Endpoint` konfiguration:</span><span class="sxs-lookup"><span data-stu-id="d9563-205">toouse a static port with WebListener, provide hello port number in hello `Endpoint` configuration:</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-weblistener-with-a-dynamic-port"></a><span data-ttu-id="d9563-206">Använda WebListener med en dynamisk port</span><span class="sxs-lookup"><span data-stu-id="d9563-206">Use WebListener with a dynamic port</span></span>
<span data-ttu-id="d9563-207">toouse en dynamiskt tilldelad port med WebListener, utelämna hello `Port` egenskap i hello `Endpoint` konfiguration:</span><span class="sxs-lookup"><span data-stu-id="d9563-207">toouse a dynamically assigned port with WebListener, omit hello `Port` property in hello `Endpoint` configuration:</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

<span data-ttu-id="d9563-208">Observera att en dynamisk port allokerats av en `Endpoint` konfiguration innehåller endast en port *per värdprocess*.</span><span class="sxs-lookup"><span data-stu-id="d9563-208">Note that a dynamic port allocated by an `Endpoint` configuration only provides one port *per host process*.</span></span> <span data-ttu-id="d9563-209">hello aktuella Service Fabric värdmodell tillåter flera service instanser och/eller repliker toobe att finns i hello samma process, vilket innebär att var och en delar samma port vid via hello hello `Endpoint` konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d9563-209">hello current Service Fabric hosting model allows multiple service instances and/or replicas toobe hosted in hello same process, meaning that each one will share hello same port when allocated through hello `Endpoint` configuration.</span></span> <span data-ttu-id="d9563-210">Flera instanser av WebListener kan dela en port med hello underliggande *http.sys* portdelning funktionen, men som inte stöds av `WebListenerCommunicationListener` på grund av toohello komplikationer införs för klientbegäranden.</span><span class="sxs-lookup"><span data-stu-id="d9563-210">Multiple WebListener instances can share a port using hello underlying *http.sys* port sharing feature, but that is not supported by `WebListenerCommunicationListener` due toohello complications it introduces for client requests.</span></span> <span data-ttu-id="d9563-211">För användning av dynamisk port är Kestrel hello rekommenderas webbservern.</span><span class="sxs-lookup"><span data-stu-id="d9563-211">For dynamic port usage, Kestrel is hello recommended web server.</span></span>

## <a name="kestrel-in-reliable-services"></a><span data-ttu-id="d9563-212">Kestrel i Reliable Services</span><span class="sxs-lookup"><span data-stu-id="d9563-212">Kestrel in Reliable Services</span></span>
<span data-ttu-id="d9563-213">Kestrel kan användas i en tillförlitlig tjänst genom att importera hello **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="d9563-213">Kestrel can be used in a Reliable Service by importing hello **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet package.</span></span> <span data-ttu-id="d9563-214">Det här paketet innehåller `KestrelCommunicationListener`, en implementering av `ICommunicationListener`, som gör att du toocreate en ASP.NET Core Webbvärden inuti en tillförlitlig tjänst med Kestrel som hello webbserver.</span><span class="sxs-lookup"><span data-stu-id="d9563-214">This package contains `KestrelCommunicationListener`, an implementation of `ICommunicationListener`, that allows you toocreate an ASP.NET Core WebHost inside a Reliable Service using Kestrel as hello web server.</span></span>

<span data-ttu-id="d9563-215">Kestrel är en plattformsoberoende webbserver för ASP.NET Core utifrån libuv, ett plattformsoberoende asynkrona i/o-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="d9563-215">Kestrel is a cross-platform web server for ASP.NET Core based on libuv, a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="d9563-216">Till skillnad från WebListener, Kestrel inte använder en centraliserad endpoint-hanteraren som *http.sys*.</span><span class="sxs-lookup"><span data-stu-id="d9563-216">Unlike WebListener, Kestrel does not use a centralized endpoint manager such as *http.sys*.</span></span> <span data-ttu-id="d9563-217">Och till skillnad från WebListener, Kestrel stöder inte portdelning mellan flera processer.</span><span class="sxs-lookup"><span data-stu-id="d9563-217">And unlike WebListener, Kestrel does not support port sharing between multiple processes.</span></span> <span data-ttu-id="d9563-218">Varje instans av Kestrel måste använda en unik port.</span><span class="sxs-lookup"><span data-stu-id="d9563-218">Each instance of Kestrel must use a unique port.</span></span>

![kestrel][4]

### <a name="kestrel-in-a-stateless-service"></a><span data-ttu-id="d9563-220">Kestrel i en tillståndslös tjänst</span><span class="sxs-lookup"><span data-stu-id="d9563-220">Kestrel in a stateless service</span></span>
<span data-ttu-id="d9563-221">toouse `Kestrel` i en tillståndslös tjänst åsidosätta hello `CreateServiceInstanceListeners` metod och returnera ett `KestrelCommunicationListener` instans:</span><span class="sxs-lookup"><span data-stu-id="d9563-221">toouse `Kestrel` in a stateless service, override hello `CreateServiceInstanceListeners` method and return a `KestrelCommunicationListener` instance:</span></span>

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

### <a name="kestrel-in-a-stateful-service"></a><span data-ttu-id="d9563-222">Kestrel i en tillståndskänslig service</span><span class="sxs-lookup"><span data-stu-id="d9563-222">Kestrel in a stateful service</span></span>
<span data-ttu-id="d9563-223">toouse `Kestrel` i en tillståndskänslig service åsidosätta hello `CreateServiceReplicaListeners` metod och returnera ett `KestrelCommunicationListener` instans:</span><span class="sxs-lookup"><span data-stu-id="d9563-223">toouse `Kestrel` in a stateful service, override hello `CreateServiceReplicaListeners` method and return a `KestrelCommunicationListener` instance:</span></span>

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

<span data-ttu-id="d9563-224">I det här exemplet är en singleton-instans av `IReliableStateManager` tillhandahålls toohello Webbvärden beroende injection behållare.</span><span class="sxs-lookup"><span data-stu-id="d9563-224">In this example, a singleton instance of `IReliableStateManager` is provided toohello WebHost dependency injection container.</span></span> <span data-ttu-id="d9563-225">Detta är inte absolut nödvändigt, men det gör att du toouse `IReliableStateManager` och tillförlitlig samlingar i åtgärden för MVC kontrollantmetoder.</span><span class="sxs-lookup"><span data-stu-id="d9563-225">This is not strictly necessary, but it allows you toouse `IReliableStateManager` and Reliable Collections in your MVC controller action methods.</span></span>

<span data-ttu-id="d9563-226">Observera att en `Endpoint` Konfigurationsnamnet **inte** tillhandahålls för`KestrelCommunicationListener` i en tillståndskänslig service.</span><span class="sxs-lookup"><span data-stu-id="d9563-226">Note that an `Endpoint` configuration name is **not** provided too`KestrelCommunicationListener` in a stateful service.</span></span> <span data-ttu-id="d9563-227">Detta förklaras i detalj i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="d9563-227">This is explained in more detail in hello following section.</span></span>

### <a name="endpoint-configuration"></a><span data-ttu-id="d9563-228">Slutpunktskonfiguration</span><span class="sxs-lookup"><span data-stu-id="d9563-228">Endpoint configuration</span></span>
<span data-ttu-id="d9563-229">En `Endpoint` konfigurationen är inte obligatoriska toouse Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d9563-229">An `Endpoint` configuration is not required toouse Kestrel.</span></span> 

<span data-ttu-id="d9563-230">Kestrel är en enkel fristående webbservern. till skillnad från WebListener (eller HttpListener), det måste inte en `Endpoint` konfigurationen i *ServiceManifest.xml* eftersom det inte kräver tidigare toostarting för URL-registrering.</span><span class="sxs-lookup"><span data-stu-id="d9563-230">Kestrel is a simple stand-alone web server; unlike WebListener (or HttpListener), it does not need an `Endpoint` configuration in *ServiceManifest.xml* because it does not require URL registration prior toostarting.</span></span> 

#### <a name="use-kestrel-with-a-static-port"></a><span data-ttu-id="d9563-231">Använda Kestrel med en statisk port</span><span class="sxs-lookup"><span data-stu-id="d9563-231">Use Kestrel with a static port</span></span>
<span data-ttu-id="d9563-232">En statisk port kan konfigureras i hello `Endpoint` konfigurationen av ServiceManifest.xml för användning med Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d9563-232">A static port can be configured in hello `Endpoint` configuration of ServiceManifest.xml for use with Kestrel.</span></span> <span data-ttu-id="d9563-233">Även om detta inte är absolut nödvändigt har två potentiella fördelar:</span><span class="sxs-lookup"><span data-stu-id="d9563-233">Although this is not strictly necessary, it provides two potential benefits:</span></span>
 1. <span data-ttu-id="d9563-234">Om hello port inte faller inom hello portintervall för programmet, öppnas den hello OS-brandväggen av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d9563-234">If hello port does not fall in hello application port range, it is opened through hello OS firewall by Service Fabric.</span></span>
 2. <span data-ttu-id="d9563-235">Hej tooyou Webbadressen som tillhandahålls via `KestrelCommunicationListener` kommer att använda den här porten.</span><span class="sxs-lookup"><span data-stu-id="d9563-235">hello URL provided tooyou through `KestrelCommunicationListener` will use this port.</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

<span data-ttu-id="d9563-236">Om en `Endpoint` är konfigurerat, filnamnet måste överföras i hello `KestrelCommunicationListener` konstruktorn:</span><span class="sxs-lookup"><span data-stu-id="d9563-236">If an `Endpoint` is configured, its name must be passed into hello `KestrelCommunicationListener` constructor:</span></span> 

```csharp
new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) => ...
```

<span data-ttu-id="d9563-237">Om en `Endpoint` används inte konfigurering, utelämna hello namn i hello `KestrelCommunicationListener` konstruktor.</span><span class="sxs-lookup"><span data-stu-id="d9563-237">If an `Endpoint` configuration is not used, omit hello name in hello `KestrelCommunicationListener` constructor.</span></span> <span data-ttu-id="d9563-238">I det här fallet används en dynamisk port.</span><span class="sxs-lookup"><span data-stu-id="d9563-238">In this case, a dynamic port will be used.</span></span> <span data-ttu-id="d9563-239">Se hello nedan för mer information.</span><span class="sxs-lookup"><span data-stu-id="d9563-239">See hello next section for more information.</span></span>

#### <a name="use-kestrel-with-a-dynamic-port"></a><span data-ttu-id="d9563-240">Använda Kestrel med en dynamisk port</span><span class="sxs-lookup"><span data-stu-id="d9563-240">Use Kestrel with a dynamic port</span></span>
<span data-ttu-id="d9563-241">Kestrel kan inte använda hello automatisk porttilldelning från hello `Endpoint` konfigurationen i ServiceManifest.xml, eftersom automatisk porttilldelning från en `Endpoint` konfigurationen tilldelar en unik port per *värd processen* , och en enda värdprocess kan innehålla flera Kestrel instanser.</span><span class="sxs-lookup"><span data-stu-id="d9563-241">Kestrel cannot use hello automatic port assignment from hello `Endpoint` configuration in ServiceManifest.xml, because automatic port assignment from an `Endpoint` configuration assigns a unique port per *host process*, and a single host process can contain multiple Kestrel instances.</span></span> <span data-ttu-id="d9563-242">Eftersom Kestrel inte stöder portdelning är fungerar detta inte som varje Kestrel-instans måste öppnas på en unik port.</span><span class="sxs-lookup"><span data-stu-id="d9563-242">Since Kestrel does not support port sharing, this does not work as each Kestrel instance must be opened on a unique port.</span></span>

<span data-ttu-id="d9563-243">toouse dynamisk porttilldelning med Kestrel, bara utelämna hello `Endpoint` konfigurationen i ServiceManifest.xml helt, och inte skickar en slutpunkt namn toohello `KestrelCommunicationListener` konstruktorn:</span><span class="sxs-lookup"><span data-stu-id="d9563-243">toouse dynamic port assignment with Kestrel, simply omit hello `Endpoint` configuration in ServiceManifest.xml entirely, and do not pass an endpoint name toohello `KestrelCommunicationListener` constructor:</span></span>

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

<span data-ttu-id="d9563-244">I den här konfigurationen `KestrelCommunicationListener` väljer automatiskt en ledig port från hello portintervall för programmet.</span><span class="sxs-lookup"><span data-stu-id="d9563-244">In this configuration, `KestrelCommunicationListener` will automatically select an unused port from hello application port range.</span></span>

## <a name="scenarios-and-configurations"></a><span data-ttu-id="d9563-245">Scenarier och konfigurationer</span><span class="sxs-lookup"><span data-stu-id="d9563-245">Scenarios and configurations</span></span>
<span data-ttu-id="d9563-246">Det här avsnittet beskriver hello följande scenarier och innehåller hello rekommenderas kombination av webbserver, portkonfiguration, alternativ för Service Fabric-integrering och övriga inställningar tooachieve en korrekt fungerande tjänst:</span><span class="sxs-lookup"><span data-stu-id="d9563-246">This section describes hello following scenarios and provides hello recommended combination of web server, port configuration, Service Fabric integration options, and miscellaneous settings tooachieve a properly functioning service:</span></span>
 - <span data-ttu-id="d9563-247">Externt exponeras ASP.NET Core tillståndslösa tjänsten</span><span class="sxs-lookup"><span data-stu-id="d9563-247">Externally exposed ASP.NET Core stateless service</span></span>
 - <span data-ttu-id="d9563-248">Endast internt ASP.NET Core tillståndslösa tjänsten</span><span class="sxs-lookup"><span data-stu-id="d9563-248">Internal-only ASP.NET Core stateless service</span></span>
 - <span data-ttu-id="d9563-249">Endast internt ASP.NET Core tillståndskänslig service</span><span class="sxs-lookup"><span data-stu-id="d9563-249">Internal-only ASP.NET Core stateful service</span></span>

<span data-ttu-id="d9563-250">En **externt exponeras** service är en som Exponerar en slutpunkt som kan nås från utanför hello kluster, vanligtvis via en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="d9563-250">An **externally exposed** service is one that exposes an endpoint reachable from outside hello cluster, usually through a load balancer.</span></span>

<span data-ttu-id="d9563-251">En **endast är interna** service är en vars endpoint kan endast nås från inom hello kluster.</span><span class="sxs-lookup"><span data-stu-id="d9563-251">An **internal-only** service is one whose endpoint is only reachable from within hello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="d9563-252">Tillståndskänslig service slutpunkter vanligtvis inte får vara exponeras toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="d9563-252">Stateful service endpoints generally should not be exposed toohello Internet.</span></span> <span data-ttu-id="d9563-253">Kluster som finns bakom en belastningsutjämnare som inte känner till Service Fabric-tjänsten upplösning, till exempel hello Azure belastningsutjämnare blir tooexpose tillståndskänsliga tjänster eftersom hello belastningsutjämnare inte ska kunna toolocate och vidarebefordra trafik toohello repliken är lämplig tillståndskänslig tjänst.</span><span class="sxs-lookup"><span data-stu-id="d9563-253">Clusters that are behind load balancers that are unaware of Service Fabric service resolution, such as hello Azure Load Balancer, will be unable tooexpose stateful services because hello load balancer will not be able toolocate and route traffic toohello appropriate stateful service replica.</span></span> 

### <a name="externally-exposed-aspnet-core-stateless-services"></a><span data-ttu-id="d9563-254">Externt exponeras ASP.NET Core tillståndslösa tjänster</span><span class="sxs-lookup"><span data-stu-id="d9563-254">Externally exposed ASP.NET Core stateless services</span></span>
<span data-ttu-id="d9563-255">WebListener är hello rekommenderas webbservern för frontend-tjänster som exponerar extern, mot Internet HTTP-slutpunkter i Windows.</span><span class="sxs-lookup"><span data-stu-id="d9563-255">WebListener is hello recommended web server for front-end services that expose external, Internet-facing HTTP endpoints on Windows.</span></span> <span data-ttu-id="d9563-256">Den ger bättre skydd mot attacker och stöder funktioner som Kestrel inte, till exempel Windows-autentisering och delning av port.</span><span class="sxs-lookup"><span data-stu-id="d9563-256">It provides better protection against attacks and supports features that Kestrel does not, such as Windows Authentication and port sharing.</span></span> 

<span data-ttu-id="d9563-257">Kestrel stöds inte som en gränsserver (Internet-inriktad) just nu.</span><span class="sxs-lookup"><span data-stu-id="d9563-257">Kestrel is not supported as an edge (Internet-facing) server at this time.</span></span> <span data-ttu-id="d9563-258">En omvänd proxy-server, till exempel IIS eller Nginx måste användas toohandle trafik från hello offentliga Internet.</span><span class="sxs-lookup"><span data-stu-id="d9563-258">A reverse proxy server such as IIS or Nginx must be used toohandle traffic from hello public Internet.</span></span>
 
<span data-ttu-id="d9563-259">När exponerade toohello Internet, en tillståndslösa tjänsten ska använda en välkänd och stabil slutpunkt som kan nås via en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="d9563-259">When exposed toohello Internet, a stateless service should use a well-known and stable endpoint that is reachable through a load balancer.</span></span> <span data-ttu-id="d9563-260">Detta är hello-URL som du kommer att ge toousers för programmet.</span><span class="sxs-lookup"><span data-stu-id="d9563-260">This is hello URL you will provide toousers of your application.</span></span> <span data-ttu-id="d9563-261">Du rekommenderas hello följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="d9563-261">hello following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="d9563-262">**Anteckningar**</span><span class="sxs-lookup"><span data-stu-id="d9563-262">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d9563-263">Webbserver</span><span class="sxs-lookup"><span data-stu-id="d9563-263">Web server</span></span> | <span data-ttu-id="d9563-264">WebListener</span><span class="sxs-lookup"><span data-stu-id="d9563-264">WebListener</span></span> | <span data-ttu-id="d9563-265">Om hello-tjänsten är bara synliga tooa betrott nätverk, ett intranät, kan Kestrel användas.</span><span class="sxs-lookup"><span data-stu-id="d9563-265">If hello service is only exposed tooa trusted network, such an intranet, Kestrel may be used.</span></span> <span data-ttu-id="d9563-266">Annars är WebListener hello önskade alternativ.</span><span class="sxs-lookup"><span data-stu-id="d9563-266">Otherwise, WebListener is hello preferred option.</span></span> |
| <span data-ttu-id="d9563-267">Portkonfiguration</span><span class="sxs-lookup"><span data-stu-id="d9563-267">Port configuration</span></span> | <span data-ttu-id="d9563-268">Statisk</span><span class="sxs-lookup"><span data-stu-id="d9563-268">static</span></span> | <span data-ttu-id="d9563-269">En välkänd statisk port ska konfigureras i hello `Endpoints` konfigurationen av ServiceManifest.xml, till exempel 80 för HTTP och 443 för HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d9563-269">A well-known static port should be configured in hello `Endpoints` configuration of ServiceManifest.xml, such as 80 for HTTP or 443 for HTTPS.</span></span> |
| <span data-ttu-id="d9563-270">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="d9563-270">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="d9563-271">Ingen</span><span class="sxs-lookup"><span data-stu-id="d9563-271">None</span></span> | <span data-ttu-id="d9563-272">Hej `ServiceFabricIntegrationOptions.None` bör användas när du konfigurerar Service Fabric-integrering mellanprogram så att hello-tjänsten inte försöker toovalidate inkommande begäranden för en unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="d9563-272">hello `ServiceFabricIntegrationOptions.None` option should be used when configuring Service Fabric integration middleware so that hello service does not attempt toovalidate incoming requests for a unique identifier.</span></span> <span data-ttu-id="d9563-273">Externa användare av ditt program vet inte hello unika identifieringsinformation används av hello mellanprogram.</span><span class="sxs-lookup"><span data-stu-id="d9563-273">External users of your application will not know hello unique identifying information used by hello middleware.</span></span> |
| <span data-ttu-id="d9563-274">Antal instanser</span><span class="sxs-lookup"><span data-stu-id="d9563-274">Instance Count</span></span> | <span data-ttu-id="d9563-275">-1</span><span class="sxs-lookup"><span data-stu-id="d9563-275">-1</span></span> | <span data-ttu-id="d9563-276">I vanliga användningsområden hello instansantalet inställningen anges för ”-1” så att en instans är tillgänglig på alla noder som tar emot trafik från en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="d9563-276">In typical use cases, hello instance count setting should be set too"-1" so that an instance is available on all nodes that receive traffic from a load balancer.</span></span> |

<span data-ttu-id="d9563-277">Om flera tjänster som externt synliga delar hello samma uppsättning noder, ska en unik men stabil URL-sökvägen användas.</span><span class="sxs-lookup"><span data-stu-id="d9563-277">If multiple externally exposed services share hello same set of nodes, a unique but stable URL path should be used.</span></span> <span data-ttu-id="d9563-278">Detta kan åstadkommas genom att ändra hello URL: en när du konfigurerar IWebHost.</span><span class="sxs-lookup"><span data-stu-id="d9563-278">This can be accomplished by modifying hello URL provided when configuring IWebHost.</span></span> <span data-ttu-id="d9563-279">Observera att detta gäller endast tooWebListener.</span><span class="sxs-lookup"><span data-stu-id="d9563-279">Note this applies tooWebListener only.</span></span>

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

### <a name="internal-only-stateless-aspnet-core-service"></a><span data-ttu-id="d9563-280">Endast internt tillståndslösa ASP.NET Core-tjänsten</span><span class="sxs-lookup"><span data-stu-id="d9563-280">Internal-only stateless ASP.NET Core service</span></span>
<span data-ttu-id="d9563-281">Tillståndslösa tjänster som endast anropas från inom hello klustret ska använda unika URL: er och dynamiskt tilldelade portar tooensure samarbete mellan flera tjänster.</span><span class="sxs-lookup"><span data-stu-id="d9563-281">Stateless services that are only called from within hello cluster should use unique URLs and dynamically assigned ports tooensure cooperation between multiple services.</span></span> <span data-ttu-id="d9563-282">Du rekommenderas hello följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="d9563-282">hello following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="d9563-283">**Anteckningar**</span><span class="sxs-lookup"><span data-stu-id="d9563-283">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d9563-284">Webbserver</span><span class="sxs-lookup"><span data-stu-id="d9563-284">Web server</span></span> | <span data-ttu-id="d9563-285">kestrel</span><span class="sxs-lookup"><span data-stu-id="d9563-285">Kestrel</span></span> | <span data-ttu-id="d9563-286">Även om WebListener kan användas för interna tillståndslösa tjänster, är Kestrel hello rekommenderade server tooallow flera service instanser tooshare en värd.</span><span class="sxs-lookup"><span data-stu-id="d9563-286">Although WebListener may be used for internal stateless services, Kestrel is hello recommended server tooallow multiple service instances tooshare a host.</span></span>  |
| <span data-ttu-id="d9563-287">Portkonfiguration</span><span class="sxs-lookup"><span data-stu-id="d9563-287">Port configuration</span></span> | <span data-ttu-id="d9563-288">dynamiskt tilldelade</span><span class="sxs-lookup"><span data-stu-id="d9563-288">dynamically assigned</span></span> | <span data-ttu-id="d9563-289">Flera repliker på en tillståndskänslig tjänst kan dela en värdprocess eller värdoperativsystemet och därför måste unika portar.</span><span class="sxs-lookup"><span data-stu-id="d9563-289">Multiple replicas of a stateful service may share a host process or host operating system and thus will need unique ports.</span></span> |
| <span data-ttu-id="d9563-290">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="d9563-290">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="d9563-291">UseUniqueServiceUrl</span><span class="sxs-lookup"><span data-stu-id="d9563-291">UseUniqueServiceUrl</span></span> | <span data-ttu-id="d9563-292">Den här inställningen förhindrar med dynamisk porttilldelning hello blivit identitet problemet som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="d9563-292">With dynamic port assignment, this setting prevents hello mistaken identity issue described earlier.</span></span> |
| <span data-ttu-id="d9563-293">InstanceCount</span><span class="sxs-lookup"><span data-stu-id="d9563-293">InstanceCount</span></span> | <span data-ttu-id="d9563-294">alla</span><span class="sxs-lookup"><span data-stu-id="d9563-294">any</span></span> | <span data-ttu-id="d9563-295">Hej instansantalet inställningen kan ställas in tooany värdet nödvändiga toooperate hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d9563-295">hello instance count setting can be set tooany value necessary toooperate hello service.</span></span> |

### <a name="internal-only-stateful-aspnet-core-service"></a><span data-ttu-id="d9563-296">Endast internt tillståndskänslig ASP.NET Core-tjänsten</span><span class="sxs-lookup"><span data-stu-id="d9563-296">Internal-only stateful ASP.NET Core service</span></span>
<span data-ttu-id="d9563-297">Tillståndskänsliga tjänster som endast anropas från inom hello klustret ska använda dynamiskt tilldelade portar tooensure samarbete mellan flera tjänster.</span><span class="sxs-lookup"><span data-stu-id="d9563-297">Stateful services that are only called from within hello cluster should use dynamically assigned ports tooensure cooperation between multiple services.</span></span> <span data-ttu-id="d9563-298">Du rekommenderas hello följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="d9563-298">hello following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="d9563-299">**Anteckningar**</span><span class="sxs-lookup"><span data-stu-id="d9563-299">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d9563-300">Webbserver</span><span class="sxs-lookup"><span data-stu-id="d9563-300">Web server</span></span> | <span data-ttu-id="d9563-301">kestrel</span><span class="sxs-lookup"><span data-stu-id="d9563-301">Kestrel</span></span> | <span data-ttu-id="d9563-302">Hej `WebListenerCommunicationListener` är inte avsedd för användning av tillståndskänsliga tjänster där repliker delar en värdprocess.</span><span class="sxs-lookup"><span data-stu-id="d9563-302">hello `WebListenerCommunicationListener` is not designed for use by stateful services in which replicas share a host process.</span></span> |
| <span data-ttu-id="d9563-303">Portkonfiguration</span><span class="sxs-lookup"><span data-stu-id="d9563-303">Port configuration</span></span> | <span data-ttu-id="d9563-304">dynamiskt tilldelade</span><span class="sxs-lookup"><span data-stu-id="d9563-304">dynamically assigned</span></span> | <span data-ttu-id="d9563-305">Flera repliker på en tillståndskänslig tjänst kan dela en värdprocess eller värdoperativsystemet och därför måste unika portar.</span><span class="sxs-lookup"><span data-stu-id="d9563-305">Multiple replicas of a stateful service may share a host process or host operating system and thus will need unique ports.</span></span> |
| <span data-ttu-id="d9563-306">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="d9563-306">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="d9563-307">UseUniqueServiceUrl</span><span class="sxs-lookup"><span data-stu-id="d9563-307">UseUniqueServiceUrl</span></span> | <span data-ttu-id="d9563-308">Den här inställningen förhindrar med dynamisk porttilldelning hello blivit identitet problemet som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="d9563-308">With dynamic port assignment, this setting prevents hello mistaken identity issue described earlier.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d9563-309">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d9563-309">Next steps</span></span>
[<span data-ttu-id="d9563-310">Felsöka Service Fabric-program med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9563-310">Debug your Service Fabric application by using Visual Studio</span></span>](service-fabric-debugging-your-application.md)

<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
