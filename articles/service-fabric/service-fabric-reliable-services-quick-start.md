---
title: "aaaCreate ditt första Service Fabric-program i C# | Microsoft Docs"
description: "Introduktion toocreating ett Microsoft Azure Service Fabric-program med tillståndslösa och tillståndskänsliga tjänster."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d9b44d75-e905-468e-b867-2190ce97379a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2017
ms.author: vturecek
ms.openlocfilehash: e95e67cc84be1b83c936b250cae9112ddc77b963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-reliable-services"></a><span data-ttu-id="3daa8-103">Kom igång med Reliable Services</span><span class="sxs-lookup"><span data-stu-id="3daa8-103">Get started with Reliable Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3daa8-104">C# i Windows</span><span class="sxs-lookup"><span data-stu-id="3daa8-104">C# on Windows</span></span>](service-fabric-reliable-services-quick-start.md)
> * [<span data-ttu-id="3daa8-105">Java i Linux</span><span class="sxs-lookup"><span data-stu-id="3daa8-105">Java on Linux</span></span>](service-fabric-reliable-services-quick-start-java.md)
> 
> 

<span data-ttu-id="3daa8-106">Ett Azure Service Fabric-program innehåller en eller flera tjänster som körs din kod.</span><span class="sxs-lookup"><span data-stu-id="3daa8-106">An Azure Service Fabric application contains one or more services that run your code.</span></span> <span data-ttu-id="3daa8-107">Den här guiden visar hur toocreate både tillståndslösa och tillståndskänsliga Service Fabric-program med [Reliable Services](service-fabric-reliable-services-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3daa8-107">This guide shows you how toocreate both stateless and stateful Service Fabric applications with [Reliable Services](service-fabric-reliable-services-introduction.md).</span></span>  <span data-ttu-id="3daa8-108">Den här videon Microsoft Virtual Academy visar också hur toocreate en tillståndslös tillförlitlig tjänst:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965"></span><span class="sxs-lookup"><span data-stu-id="3daa8-108">This Microsoft Virtual Academy video also shows you how toocreate a stateless Reliable service: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965"></span></span>  
<img src="./media/service-fabric-reliable-services-quick-start/ReliableServicesVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="basic-concepts"></a><span data-ttu-id="3daa8-109">Grundläggande begrepp</span><span class="sxs-lookup"><span data-stu-id="3daa8-109">Basic concepts</span></span>
<span data-ttu-id="3daa8-110">tooget igång med tillförlitlig tjänster kan du bara behöver toounderstand några grundläggande begrepp:</span><span class="sxs-lookup"><span data-stu-id="3daa8-110">tooget started with Reliable Services, you only need toounderstand a few basic concepts:</span></span>

* <span data-ttu-id="3daa8-111">**Typen tjänst**: det här är din implementering.</span><span class="sxs-lookup"><span data-stu-id="3daa8-111">**Service type**: This is your service implementation.</span></span> <span data-ttu-id="3daa8-112">Den definieras av hello-klass som du skriver och som utökar `StatelessService` och annan kod eller beroenden, används tillsammans med ett namn och ett versionsnummer.</span><span class="sxs-lookup"><span data-stu-id="3daa8-112">It is defined by hello class you write that extends `StatelessService` and any other code or dependencies used therein, along with a name and a version number.</span></span>
* <span data-ttu-id="3daa8-113">**Med namnet tjänstinstans**: toorun din tjänst du skapar namngivna instanser av service-typen mycket som du skapar objektinstanser av en klasstyp.</span><span class="sxs-lookup"><span data-stu-id="3daa8-113">**Named service instance**: toorun your service, you create named instances of your service type, much like you create object instances of a class type.</span></span> <span data-ttu-id="3daa8-114">En tjänstinstans har ett namn i hello form av en URI med hello ”fabric: /” schemat, till exempel ”fabric: / MyApp/MyService”.</span><span class="sxs-lookup"><span data-stu-id="3daa8-114">A service instance has a name in hello form of a URI using hello "fabric:/" scheme, such as "fabric:/MyApp/MyService".</span></span>
* <span data-ttu-id="3daa8-115">**Tjänstvärden**: hello med namnet tjänstinstanser som du skapar måste toorun inom en värdprocess.</span><span class="sxs-lookup"><span data-stu-id="3daa8-115">**Service host**: hello named service instances you create need toorun inside a host process.</span></span> <span data-ttu-id="3daa8-116">hello tjänstvärden är bara en process där instanser av tjänsten kan köras.</span><span class="sxs-lookup"><span data-stu-id="3daa8-116">hello service host is just a process where instances of your service can run.</span></span>
* <span data-ttu-id="3daa8-117">**Registrering av tjänst**: registrering sammanför allt.</span><span class="sxs-lookup"><span data-stu-id="3daa8-117">**Service registration**: Registration brings everything together.</span></span> <span data-ttu-id="3daa8-118">hello service-typen måste vara registrerad med hello Service Fabric runtime i en tjänst värd tooallow Service Fabric toocreate instanser av den toorun.</span><span class="sxs-lookup"><span data-stu-id="3daa8-118">hello service type must be registered with hello Service Fabric runtime in a service host tooallow Service Fabric toocreate instances of it toorun.</span></span>  

## <a name="create-a-stateless-service"></a><span data-ttu-id="3daa8-119">Skapa en tillståndslös tjänst</span><span class="sxs-lookup"><span data-stu-id="3daa8-119">Create a stateless service</span></span>
<span data-ttu-id="3daa8-120">En tillståndslös tjänst är en typ av tjänst som används för närvarande hello normen i molnprogram.</span><span class="sxs-lookup"><span data-stu-id="3daa8-120">A stateless service is a type of service that is currently hello norm in cloud applications.</span></span> <span data-ttu-id="3daa8-121">Anses tillståndslös eftersom själva hello-tjänsten inte innehåller data som behöver toobe lagras på ett tillförlitligt sätt eller hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="3daa8-121">It is considered stateless because hello service itself does not contain data that needs toobe stored reliably or made highly available.</span></span> <span data-ttu-id="3daa8-122">Om en instans av en tillståndslösa tjänsten stängs av, förloras alla det interna tillståndet.</span><span class="sxs-lookup"><span data-stu-id="3daa8-122">If an instance of a stateless service shuts down, all of its internal state is lost.</span></span> <span data-ttu-id="3daa8-123">I den här typen av tjänsten tillstånd måste vara beständiga tooan extern butik, till exempel Azure-tabeller eller en SQL-databas i den toobe gjort hög tillgänglighet och tillförlitlig.</span><span class="sxs-lookup"><span data-stu-id="3daa8-123">In this type of service, state must be persisted tooan external store, such as Azure Tables or a SQL database, for it toobe made highly available and reliable.</span></span>

<span data-ttu-id="3daa8-124">Starta Visual Studio 2015 eller Visual Studio 2017 som administratör och skapa ett nytt Service Fabric application projekt med namnet *HelloWorld*:</span><span class="sxs-lookup"><span data-stu-id="3daa8-124">Launch Visual Studio 2015 or Visual Studio 2017 as an administrator, and create a new Service Fabric application project named *HelloWorld*:</span></span>

![Använd hello nytt projekt dialogrutan rutan toocreate ett nytt Service Fabric-program](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject.png)

<span data-ttu-id="3daa8-126">Skapa ett projekt för tillståndslösa tjänsten med namnet *HelloWorldStateless*:</span><span class="sxs-lookup"><span data-stu-id="3daa8-126">Then create a stateless service project named *HelloWorldStateless*:</span></span>

![Skapa ett projekt för tillståndslösa tjänsten i hello andra dialogrutan](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject2.png)

<span data-ttu-id="3daa8-128">Din lösning innehåller nu två projekt:</span><span class="sxs-lookup"><span data-stu-id="3daa8-128">Your solution now contains two projects:</span></span>

* <span data-ttu-id="3daa8-129">*HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="3daa8-129">*HelloWorld*.</span></span> <span data-ttu-id="3daa8-130">Detta är hello *programmet* projekt som innehåller din *services*.</span><span class="sxs-lookup"><span data-stu-id="3daa8-130">This is hello *application* project that contains your *services*.</span></span> <span data-ttu-id="3daa8-131">Den innehåller också hello programmanifestet som beskriver hello program, samt ett antal PowerShell-skript som hjälper dig toodeploy ditt program.</span><span class="sxs-lookup"><span data-stu-id="3daa8-131">It also contains hello application manifest that describes hello application, as well as a number of PowerShell scripts that help you toodeploy your application.</span></span>
* <span data-ttu-id="3daa8-132">*HelloWorldStateless*.</span><span class="sxs-lookup"><span data-stu-id="3daa8-132">*HelloWorldStateless*.</span></span> <span data-ttu-id="3daa8-133">Detta är hello service-projekt.</span><span class="sxs-lookup"><span data-stu-id="3daa8-133">This is hello service project.</span></span> <span data-ttu-id="3daa8-134">Den innehåller hello tillståndslösa tjänsten implementering.</span><span class="sxs-lookup"><span data-stu-id="3daa8-134">It contains hello stateless service implementation.</span></span>

## <a name="implement-hello-service"></a><span data-ttu-id="3daa8-135">Implementera hello-tjänsten</span><span class="sxs-lookup"><span data-stu-id="3daa8-135">Implement hello service</span></span>
<span data-ttu-id="3daa8-136">Öppna hello **HelloWorldStateless.cs** filen i hello service-projekt.</span><span class="sxs-lookup"><span data-stu-id="3daa8-136">Open hello **HelloWorldStateless.cs** file in hello service project.</span></span> <span data-ttu-id="3daa8-137">En tjänst kan köra all affärslogik i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3daa8-137">In Service Fabric, a service can run any business logic.</span></span> <span data-ttu-id="3daa8-138">hello service API innehåller två startpunkter för din kod:</span><span class="sxs-lookup"><span data-stu-id="3daa8-138">hello service API provides two entry points for your code:</span></span>

* <span data-ttu-id="3daa8-139">En öppen metoden, kallas *RunAsync*, där du kan börja köra alla arbetsbelastningar, inklusive tidskrävande beräkning av arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="3daa8-139">An open-ended entry point method, called *RunAsync*, where you can begin executing any workloads, including long-running compute workloads.</span></span>

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}
```

* <span data-ttu-id="3daa8-140">En kommunikation startpunkt där du kan ansluta din kommunikation stack föredrar, till exempel ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3daa8-140">A communication entry point where you can plug in your communication stack of choice, such as ASP.NET Core.</span></span> <span data-ttu-id="3daa8-141">Det är där du kan börja ta emot begäranden från användare och andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="3daa8-141">This is where you can start receiving requests from users and other services.</span></span>

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}
```

<span data-ttu-id="3daa8-142">I den här självstudiekursen fokuserar vi på hello `RunAsync()` metoden.</span><span class="sxs-lookup"><span data-stu-id="3daa8-142">In this tutorial, we will focus on hello `RunAsync()` entry point method.</span></span> <span data-ttu-id="3daa8-143">Det är där du kan starta direkt köra din kod.</span><span class="sxs-lookup"><span data-stu-id="3daa8-143">This is where you can immediately start running your code.</span></span>
<span data-ttu-id="3daa8-144">hello projektmall innehåller ett exempel på implementering av `RunAsync()` som ökar en löpande uppräkning.</span><span class="sxs-lookup"><span data-stu-id="3daa8-144">hello project template includes a sample implementation of `RunAsync()` that increments a rolling count.</span></span>

> [!NOTE]
> <span data-ttu-id="3daa8-145">Mer information om hur toowork med en kommunikation stacken finns [Service Fabric Web API-tjänster med OWIN själva värd](service-fabric-reliable-services-communication-webapi.md)</span><span class="sxs-lookup"><span data-stu-id="3daa8-145">For details about how toowork with a communication stack, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md)</span></span>
> 
> 

### <a name="runasync"></a><span data-ttu-id="3daa8-146">RunAsync</span><span class="sxs-lookup"><span data-stu-id="3daa8-146">RunAsync</span></span>
```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace hello following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    long iterations = 0;

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        ServiceEventSource.Current.ServiceMessage(this, "Working-{0}", ++iterations);

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
}
```

<span data-ttu-id="3daa8-147">hello plattform anropar den här metoden när en instans av en tjänst är monterade och klar tooexecute.</span><span class="sxs-lookup"><span data-stu-id="3daa8-147">hello platform calls this method when an instance of a service is placed and ready tooexecute.</span></span> <span data-ttu-id="3daa8-148">För en tillståndslös tjänst innebär som bara när hello tjänstinstans öppnas.</span><span class="sxs-lookup"><span data-stu-id="3daa8-148">For a stateless service, that simply means when hello service instance is opened.</span></span> <span data-ttu-id="3daa8-149">En token för annullering tillhandahålls toocoordinate när service-instans måste toobe stängd.</span><span class="sxs-lookup"><span data-stu-id="3daa8-149">A cancellation token is provided toocoordinate when your service instance needs toobe closed.</span></span> <span data-ttu-id="3daa8-150">I Service Fabric inträffa cykeln Öppna/stänga av en tjänstinstans många gånger under hello livslängd för hello-tjänsten som helhet.</span><span class="sxs-lookup"><span data-stu-id="3daa8-150">In Service Fabric, this open/close cycle of a service instance can occur many times over hello lifetime of hello service as a whole.</span></span> <span data-ttu-id="3daa8-151">Detta kan inträffa av olika orsaker, bland annat:</span><span class="sxs-lookup"><span data-stu-id="3daa8-151">This can happen for various reasons, including:</span></span>

* <span data-ttu-id="3daa8-152">hello flyttas instanser av tjänsten för resurser.</span><span class="sxs-lookup"><span data-stu-id="3daa8-152">hello system moves your service instances for resource balancing.</span></span>
* <span data-ttu-id="3daa8-153">Fel uppstår i koden.</span><span class="sxs-lookup"><span data-stu-id="3daa8-153">Faults occur in your code.</span></span>
* <span data-ttu-id="3daa8-154">hello programmets eller systemets uppgraderas.</span><span class="sxs-lookup"><span data-stu-id="3daa8-154">hello application or system is upgraded.</span></span>
* <span data-ttu-id="3daa8-155">hello underliggande maskinvara påträffar ett avbrott.</span><span class="sxs-lookup"><span data-stu-id="3daa8-155">hello underlying hardware experiences an outage.</span></span>

<span data-ttu-id="3daa8-156">Den här orchestration hanteras av hello system tookeep tjänsten hög tillgänglighet och korrekt Balanserat.</span><span class="sxs-lookup"><span data-stu-id="3daa8-156">This orchestration is managed by hello system tookeep your service highly available and properly balanced.</span></span>

<span data-ttu-id="3daa8-157">`RunAsync()`bör inte blockera synkront.</span><span class="sxs-lookup"><span data-stu-id="3daa8-157">`RunAsync()` should not block synchronously.</span></span> <span data-ttu-id="3daa8-158">Implementeringen av RunAsync ska returnera en uppgift eller await på alla tidskrävande eller blockerar åtgärder tooallow hello runtime toocontinue.</span><span class="sxs-lookup"><span data-stu-id="3daa8-158">Your implementation of RunAsync should return a Task or await on any long-running or blocking operations tooallow hello runtime toocontinue.</span></span> <span data-ttu-id="3daa8-159">Notera i hello `while(true)` loop i hello föregående exempel är en åtgärdsreturnerande `await Task.Delay()` används.</span><span class="sxs-lookup"><span data-stu-id="3daa8-159">Note in hello `while(true)` loop in hello previous example, a Task-returning `await Task.Delay()` is used.</span></span> <span data-ttu-id="3daa8-160">Om din arbetsbelastning måste blockerar synkront kan du schemalägga en ny uppgift med `Task.Run()` i din `RunAsync` implementering.</span><span class="sxs-lookup"><span data-stu-id="3daa8-160">If your workload must block synchronously, you should schedule a new Task with `Task.Run()` in your `RunAsync` implementation.</span></span>

<span data-ttu-id="3daa8-161">Annullering av din arbetsbelastning är en samverkande ansträngning styrd av hello tillhandahålls annullering token.</span><span class="sxs-lookup"><span data-stu-id="3daa8-161">Cancellation of your workload is a cooperative effort orchestrated by hello provided cancellation token.</span></span> <span data-ttu-id="3daa8-162">hello systemet väntar på uppgiften-tooend (som lyckades, avbokning eller fel) innan den flyttas på.</span><span class="sxs-lookup"><span data-stu-id="3daa8-162">hello system will wait for your task tooend (by successful completion, cancellation, or fault) before it moves on.</span></span> <span data-ttu-id="3daa8-163">Det är viktigt toohonor hello annullering token, Slutför någon fungerar och avsluta `RunAsync()` så snabbt som möjligt när hello systemet begär annullering.</span><span class="sxs-lookup"><span data-stu-id="3daa8-163">It is important toohonor hello cancellation token, finish any work, and exit `RunAsync()` as quickly as possible when hello system requests cancellation.</span></span>

<span data-ttu-id="3daa8-164">I det här exemplet tillståndslösa tjänsten lagras hello antal i en lokal variabel.</span><span class="sxs-lookup"><span data-stu-id="3daa8-164">In this stateless service example, hello count is stored in a local variable.</span></span> <span data-ttu-id="3daa8-165">Men eftersom detta är en tillståndslös tjänst hello-värde som lagras finns bara för hello aktuella livscykeln för dess tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="3daa8-165">But because this is a stateless service, hello value that's stored exists only for hello current lifecycle of its service instance.</span></span> <span data-ttu-id="3daa8-166">När tjänsten hello flyttas eller startas om, går hello värdet förlorad.</span><span class="sxs-lookup"><span data-stu-id="3daa8-166">When hello service moves or restarts, hello value is lost.</span></span>

## <a name="create-a-stateful-service"></a><span data-ttu-id="3daa8-167">Skapa en tillståndskänslig tjänst</span><span class="sxs-lookup"><span data-stu-id="3daa8-167">Create a stateful service</span></span>
<span data-ttu-id="3daa8-168">Service Fabric introducerar en ny typ av tjänst som är tillståndskänslig.</span><span class="sxs-lookup"><span data-stu-id="3daa8-168">Service Fabric introduces a new kind of service that is stateful.</span></span> <span data-ttu-id="3daa8-169">En tillståndskänslig service kan upprätthålla tillstånd på ett tillförlitligt sätt inom hello själva tjänsten, samordnad med hello-kod som använder den.</span><span class="sxs-lookup"><span data-stu-id="3daa8-169">A stateful service can maintain state reliably within hello service itself, co-located with hello code that's using it.</span></span> <span data-ttu-id="3daa8-170">Tillstånd görs högtillgänglig av Service Fabric utan hello måste toopersist tillstånd tooan extern butik.</span><span class="sxs-lookup"><span data-stu-id="3daa8-170">State is made highly available by Service Fabric without hello need toopersist state tooan external store.</span></span>

<span data-ttu-id="3daa8-171">tooconvert ett värde för prestandaräknaren från tillståndslös toohighly tillgänglig och beständiga när hello service flyttas eller startas om, behöver du en tillståndskänslig tjänst.</span><span class="sxs-lookup"><span data-stu-id="3daa8-171">tooconvert a counter value from stateless toohighly available and persistent, even when hello service moves or restarts, you need a stateful service.</span></span>

<span data-ttu-id="3daa8-172">Hej i samma *HelloWorld* programmet, du kan lägga till en ny tjänst genom att högerklicka på hello Services referenser i hello projektet och välja **Lägg till ny tjänst för Service Fabric ->**.</span><span class="sxs-lookup"><span data-stu-id="3daa8-172">In hello same *HelloWorld* application, you can add a new service by right-clicking on hello Services references in hello application project and selecting **Add ->  New Service Fabric Service**.</span></span>

![Lägg till en service tooyour Service Fabric-program](media/service-fabric-reliable-services-quick-start/hello-stateful-NewService.png)

<span data-ttu-id="3daa8-174">Välj **tillståndskänslig Service** och ger den namnet *HelloWorldStateful*.</span><span class="sxs-lookup"><span data-stu-id="3daa8-174">Select **Stateful Service** and name it *HelloWorldStateful*.</span></span> <span data-ttu-id="3daa8-175">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3daa8-175">Click **OK**.</span></span>

![Använd hello nytt projekt dialogrutan rutan toocreate en ny tillståndskänslig Service Fabric-tjänst](media/service-fabric-reliable-services-quick-start/hello-stateful-NewProject.png)

<span data-ttu-id="3daa8-177">Ditt program bör nu ha två tjänster: hello tillståndslösa tjänsten *HelloWorldStateless* och hello tillståndskänslig service *HelloWorldStateful*.</span><span class="sxs-lookup"><span data-stu-id="3daa8-177">Your application should now have two services: hello stateless service *HelloWorldStateless* and hello stateful service *HelloWorldStateful*.</span></span>

<span data-ttu-id="3daa8-178">En tillståndskänslig tjänst har hello samma startpunkter som en tillståndslös tjänst.</span><span class="sxs-lookup"><span data-stu-id="3daa8-178">A stateful service has hello same entry points as a stateless service.</span></span> <span data-ttu-id="3daa8-179">hello största skillnaden är hello tillgängligheten för en *tillståndsprovidern* som kan lagra tillstånd på ett tillförlitligt sätt.</span><span class="sxs-lookup"><span data-stu-id="3daa8-179">hello main difference is hello availability of a *state provider* that can store state reliably.</span></span> <span data-ttu-id="3daa8-180">Service Fabric medföljer en implementering av provider för tillstånd kallas [tillförlitliga samlingar](service-fabric-reliable-services-reliable-collections.md), vilket kan du skapa replikerade datastrukturer via hello tillförlitliga Tillståndshanterare.</span><span class="sxs-lookup"><span data-stu-id="3daa8-180">Service Fabric comes with a state provider implementation called [Reliable Collections](service-fabric-reliable-services-reliable-collections.md), which lets you create replicated data structures through hello Reliable State Manager.</span></span> <span data-ttu-id="3daa8-181">En tillståndskänslig tillförlitlig tjänst använder den här tillståndsprovidern som standard.</span><span class="sxs-lookup"><span data-stu-id="3daa8-181">A stateful Reliable Service uses this state provider by default.</span></span>

<span data-ttu-id="3daa8-182">Öppna **HelloWorldStateful.cs** i *HelloWorldStateful*, som innehåller hello följande RunAsync-metoden:</span><span class="sxs-lookup"><span data-stu-id="3daa8-182">Open **HelloWorldStateful.cs** in *HelloWorldStateful*, which contains hello following RunAsync method:</span></span>

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace hello following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        using (var tx = this.StateManager.CreateTransaction())
        {
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");

            ServiceEventSource.Current.ServiceMessage(this, "Current Counter Value: {0}",
                result.HasValue ? result.Value.ToString() : "Value does not exist.");

            await myDictionary.AddOrUpdateAsync(tx, "Counter", 0, (key, value) => ++value);

            // If an exception is thrown before calling CommitAsync, hello transaction aborts, all changes are
            // discarded, and nothing is saved toohello secondary replicas.
            await tx.CommitAsync();
        }

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
```

### <a name="runasync"></a><span data-ttu-id="3daa8-183">RunAsync</span><span class="sxs-lookup"><span data-stu-id="3daa8-183">RunAsync</span></span>
<span data-ttu-id="3daa8-184">`RunAsync()`fungerar på samma sätt i tillståndskänsliga och tillståndslösa tjänster.</span><span class="sxs-lookup"><span data-stu-id="3daa8-184">`RunAsync()` operates similarly in stateful and stateless services.</span></span> <span data-ttu-id="3daa8-185">Dock i en tillståndskänslig service hello plattform utför ytterligare arbete för din räkning innan den kör `RunAsync()`.</span><span class="sxs-lookup"><span data-stu-id="3daa8-185">However, in a stateful service, hello platform performs additional work on your behalf before it executes `RunAsync()`.</span></span> <span data-ttu-id="3daa8-186">Detta verk kan inkludera att säkerställa att hello tillförlitliga Tillståndshanterare och tillförlitlig samlingar är klar toouse.</span><span class="sxs-lookup"><span data-stu-id="3daa8-186">This work can include ensuring that hello Reliable State Manager and Reliable Collections are ready toouse.</span></span>

### <a name="reliable-collections-and-hello-reliable-state-manager"></a><span data-ttu-id="3daa8-187">Tillförlitliga samlingar och hello tillförlitliga Tillståndshanterare</span><span class="sxs-lookup"><span data-stu-id="3daa8-187">Reliable Collections and hello Reliable State Manager</span></span>
```csharp
var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
```

<span data-ttu-id="3daa8-188">[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) är en dictionary-implementering som du kan använda tooreliably arkivtillståndet i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3daa8-188">[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) is a dictionary implementation that you can use tooreliably store state in hello service.</span></span> <span data-ttu-id="3daa8-189">Med Service Fabric och tillförlitlig samlingar kan du lagra data direkt i din tjänst utan hello behovet av en extern beständiga arkivet.</span><span class="sxs-lookup"><span data-stu-id="3daa8-189">With Service Fabric and Reliable Collections, you can store data directly in your service without hello need for an external persistent store.</span></span> <span data-ttu-id="3daa8-190">Tillförlitliga samlingar gör att dina data är hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="3daa8-190">Reliable Collections make your data highly available.</span></span> <span data-ttu-id="3daa8-191">Service Fabric åstadkommer detta genom att skapa och hantera flera *repliker* av tjänsten för dig.</span><span class="sxs-lookup"><span data-stu-id="3daa8-191">Service Fabric accomplishes this by creating and managing multiple *replicas* of your service for you.</span></span> <span data-ttu-id="3daa8-192">Det ger också en API som avlägsnar bort hello svårigheter för att hantera dessa repliker och deras tillståndsövergångar.</span><span class="sxs-lookup"><span data-stu-id="3daa8-192">It also provides an API that abstracts away hello complexities of managing those replicas and their state transitions.</span></span>

<span data-ttu-id="3daa8-193">Tillförlitliga samlingar kan lagra alla .NET-typ, inklusive din anpassade typer med några varningar.</span><span class="sxs-lookup"><span data-stu-id="3daa8-193">Reliable Collections can store any .NET type, including your custom types, with a couple of caveats:</span></span>

* <span data-ttu-id="3daa8-194">Service Fabric gör ditt tillstånd hög tillgänglighet av *replikerar* tillstånd mellan noder och tillförlitlig samlingar lagra data toolocal disken på varje replik.</span><span class="sxs-lookup"><span data-stu-id="3daa8-194">Service Fabric makes your state highly available by *replicating* state across nodes, and Reliable Collections store your data toolocal disk on each replica.</span></span> <span data-ttu-id="3daa8-195">Det innebär att allt som lagras i tillförlitliga samlingar måste vara *serialiserbara*.</span><span class="sxs-lookup"><span data-stu-id="3daa8-195">This means that everything that is stored in Reliable Collections must be *serializable*.</span></span> <span data-ttu-id="3daa8-196">Som standard använder tillförlitliga samlingar [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) för serialisering, så det är viktigt toomake till att din typer är [stöds av hello datakontraktserialiserren](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) när du använder hello standard serialiserare.</span><span class="sxs-lookup"><span data-stu-id="3daa8-196">By default, Reliable Collections use [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) for serialization, so it's important toomake sure that your types are [supported by hello Data Contract Serializer](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) when you use hello default serializer.</span></span>
* <span data-ttu-id="3daa8-197">Objekt replikeras för hög tillgänglighet när du gör transaktioner på tillförlitliga samlingar.</span><span class="sxs-lookup"><span data-stu-id="3daa8-197">Objects are replicated for high availability when you commit transactions on Reliable Collections.</span></span> <span data-ttu-id="3daa8-198">Objekt som lagras i tillförlitliga samlingar sparas i lokalt minne i din tjänst.</span><span class="sxs-lookup"><span data-stu-id="3daa8-198">Objects stored in Reliable Collections are kept in local memory in your service.</span></span> <span data-ttu-id="3daa8-199">Det innebär att du har en lokal referens toohello objekt.</span><span class="sxs-lookup"><span data-stu-id="3daa8-199">This means that you have a local reference toohello object.</span></span>
  
   <span data-ttu-id="3daa8-200">Det är viktigt att du inte mutera lokala instanser av dessa objekt utan att utföra en uppdateringsåtgärd på hello tillförlitliga samling i en transaktion.</span><span class="sxs-lookup"><span data-stu-id="3daa8-200">It is important that you do not mutate local instances of those objects without performing an update operation on hello reliable collection in a transaction.</span></span> <span data-ttu-id="3daa8-201">Det beror på att ändringarna toolocal objektinstanser inte replikeras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3daa8-201">This is because changes toolocal instances of objects will not be replicated automatically.</span></span> <span data-ttu-id="3daa8-202">Du måste infoga igen hello objektet tillbaka till hello ordlista eller Använd en av hello *uppdatera* metoder i hello ordlistan.</span><span class="sxs-lookup"><span data-stu-id="3daa8-202">You must re-insert hello object back into hello dictionary or use one of hello *update* methods on hello dictionary.</span></span>

<span data-ttu-id="3daa8-203">hello tillförlitliga Tillståndshanterare hanterar tillförlitliga samlingar åt dig.</span><span class="sxs-lookup"><span data-stu-id="3daa8-203">hello Reliable State Manager manages Reliable Collections for you.</span></span> <span data-ttu-id="3daa8-204">Du kan bara begära hello tillförlitliga Tillståndshanterare för en tillförlitlig samling efter namn när som helst och var som helst i din tjänst.</span><span class="sxs-lookup"><span data-stu-id="3daa8-204">You can simply ask hello Reliable State Manager for a reliable collection by name at any time and at any place in your service.</span></span> <span data-ttu-id="3daa8-205">hello tillförlitliga Tillståndshanterare garanterar att du får en referens tillbaka.</span><span class="sxs-lookup"><span data-stu-id="3daa8-205">hello Reliable State Manager ensures that you get a reference back.</span></span> <span data-ttu-id="3daa8-206">Inte rekommenderar vi att du sparar referenser tooreliable samling instanser i klassmedlemsvariabler eller egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3daa8-206">We don't recommended that you save references tooreliable collection instances in class member variables or properties.</span></span> <span data-ttu-id="3daa8-207">Särskild försiktighet måste vidtas tooensure hello referens har angetts tooan instans alltid i hello service livscykel.</span><span class="sxs-lookup"><span data-stu-id="3daa8-207">Special care must be taken tooensure that hello reference is set tooan instance at all times in hello service lifecycle.</span></span> <span data-ttu-id="3daa8-208">hello tillförlitliga Tillståndshanterare hanterar det här fungerar och har optimerats för Upprepa besök.</span><span class="sxs-lookup"><span data-stu-id="3daa8-208">hello Reliable State Manager handles this work for you, and it's optimized for repeat visits.</span></span>

### <a name="transactional-and-asynchronous-operations"></a><span data-ttu-id="3daa8-209">Transaktionell och asynkrona åtgärder</span><span class="sxs-lookup"><span data-stu-id="3daa8-209">Transactional and asynchronous operations</span></span>
```C#
using (ITransaction tx = this.StateManager.CreateTransaction())
{
    var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");

    await myDictionary.AddOrUpdateAsync(tx, "Counter-1", 0, (k, v) => ++v);

    await tx.CommitAsync();
}
```

<span data-ttu-id="3daa8-210">Tillförlitliga samlingar har många hello samma åtgärder som deras `System.Collections.Generic` och `System.Collections.Concurrent` motsvarigheter göra, förutom LINQ.</span><span class="sxs-lookup"><span data-stu-id="3daa8-210">Reliable Collections have many of hello same operations that their `System.Collections.Generic` and `System.Collections.Concurrent` counterparts do, except LINQ.</span></span> <span data-ttu-id="3daa8-211">Åtgärder på tillförlitliga samlingar är asynkron.</span><span class="sxs-lookup"><span data-stu-id="3daa8-211">Operations on Reliable Collections are asynchronous.</span></span> <span data-ttu-id="3daa8-212">Detta är eftersom skrivåtgärder med tillförlitlig samlingar utföra tooreplicate för i/o-åtgärder och bevara data toodisk.</span><span class="sxs-lookup"><span data-stu-id="3daa8-212">This is because write operations with Reliable Collections perform I/O operations tooreplicate and persist data toodisk.</span></span>

<span data-ttu-id="3daa8-213">Åtgärder för insamling av tillförlitliga är *transaktionella*, så att du kan behålla tillstånd konsekvent över flera tillförlitliga samlingar och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="3daa8-213">Reliable Collection operations are *transactional*, so that you can keep state consistent across multiple Reliable Collections and operations.</span></span> <span data-ttu-id="3daa8-214">Du kan till exempel ett arbetsobjekt från en tillförlitlig kö har status Created, utföra en åtgärd på det och spara hello resultat i en tillförlitlig Dictionary alla inom en enskild transaktion.</span><span class="sxs-lookup"><span data-stu-id="3daa8-214">For example, you may dequeue a work item from a Reliable Queue, perform an operation on it, and save hello result in a Reliable Dictionary, all within a single transaction.</span></span> <span data-ttu-id="3daa8-215">Detta behandlas som en atomisk åtgärd och det garanterar hello hela åtgärden lyckas eller hello hela åtgärden återställs.</span><span class="sxs-lookup"><span data-stu-id="3daa8-215">This is treated as an atomic operation, and it guarantees that either hello entire operation will succeed or hello entire operation will roll back.</span></span> <span data-ttu-id="3daa8-216">Om ett fel uppstår när du har status Created hello objektet men innan du sparar hello resultatet hello hela transaktionen återställs och hello objekt förblir i hello kö för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="3daa8-216">If an error occurs after you dequeue hello item but before you save hello result, hello entire transaction is rolled back and hello item remains in hello queue for processing.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="3daa8-217">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="3daa8-217">Run hello application</span></span>
<span data-ttu-id="3daa8-218">Returnerar vi nu toohello *HelloWorld* program.</span><span class="sxs-lookup"><span data-stu-id="3daa8-218">We now return toohello *HelloWorld* application.</span></span> <span data-ttu-id="3daa8-219">Du kan nu skapa och distribuera tjänster.</span><span class="sxs-lookup"><span data-stu-id="3daa8-219">You can now build and deploy your services.</span></span> <span data-ttu-id="3daa8-220">När du trycker på **F5**, ditt program kommer att byggas och distribuerade tooyour lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="3daa8-220">When you press **F5**, your application will be built and deployed tooyour local cluster.</span></span>

<span data-ttu-id="3daa8-221">När hello-tjänster startar körs, kan du visa hello genereras ETW Event Tracing for Windows ()-händelser i en **diagnostiska händelser** fönster.</span><span class="sxs-lookup"><span data-stu-id="3daa8-221">After hello services start running, you can view hello generated Event Tracing for Windows (ETW) events in a **Diagnostic Events** window.</span></span> <span data-ttu-id="3daa8-222">Observera att hello händelser visas från både hello tillståndslösa tjänsten och hello tillståndskänslig service i hello program.</span><span class="sxs-lookup"><span data-stu-id="3daa8-222">Note that hello events displayed are from both hello stateless service and hello stateful service in hello application.</span></span> <span data-ttu-id="3daa8-223">Du kan pausa hello dataström genom att klicka på hello **pausa** knappen.</span><span class="sxs-lookup"><span data-stu-id="3daa8-223">You can pause hello stream by clicking hello **Pause** button.</span></span> <span data-ttu-id="3daa8-224">Du kan sedan kontrollera hello information om ett meddelande genom att expandera meddelandet.</span><span class="sxs-lookup"><span data-stu-id="3daa8-224">You can then examine hello details of a message by expanding that message.</span></span>

> [!NOTE]
> <span data-ttu-id="3daa8-225">Innan du kör programmet hello kan du kontrollera att du har en lokal utveckling kluster som kör.</span><span class="sxs-lookup"><span data-stu-id="3daa8-225">Before you run hello application, make sure that you have a local development cluster running.</span></span> <span data-ttu-id="3daa8-226">Kolla in hello [Kom igång med](service-fabric-get-started.md) information om hur du konfigurerar din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="3daa8-226">Check out hello [getting started guide](service-fabric-get-started.md) for information on setting up your local environment.</span></span>
> 
> 

![Visa diagnostik händelser i Visual Studio](media/service-fabric-reliable-services-quick-start/hello-stateful-Output.png)

## <a name="next-steps"></a><span data-ttu-id="3daa8-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3daa8-228">Next steps</span></span>
[<span data-ttu-id="3daa8-229">Felsöka Service Fabric-program i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3daa8-229">Debug your Service Fabric application in Visual Studio</span></span>](service-fabric-debugging-your-application.md)

[<span data-ttu-id="3daa8-230">Komma igång: Service Fabric Web API-tjänster med OWIN själva värd</span><span class="sxs-lookup"><span data-stu-id="3daa8-230">Get started: Service Fabric Web API services with OWIN self-hosting</span></span>](service-fabric-reliable-services-communication-webapi.md)

[<span data-ttu-id="3daa8-231">Mer information om tillförlitlig samlingar</span><span class="sxs-lookup"><span data-stu-id="3daa8-231">Learn more about Reliable Collections</span></span>](service-fabric-reliable-services-reliable-collections.md)

[<span data-ttu-id="3daa8-232">Distribuera ett program</span><span class="sxs-lookup"><span data-stu-id="3daa8-232">Deploy an application</span></span>](service-fabric-deploy-remove-applications.md)

[<span data-ttu-id="3daa8-233">Uppgradering av programmet</span><span class="sxs-lookup"><span data-stu-id="3daa8-233">Application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="3daa8-234">För utvecklare för Reliable Services</span><span class="sxs-lookup"><span data-stu-id="3daa8-234">Developer reference for Reliable Services</span></span>](https://msdn.microsoft.com/library/azure/dn706529.aspx)

