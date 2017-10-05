---
title: "Skapa din första Service Fabric-program i C# | Microsoft Docs"
description: "Introduktion till att skapa ett Microsoft Azure Service Fabric-program med tillståndslösa och tillståndskänsliga tjänster."
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
ms.openlocfilehash: 813021d6239ae3cf79bb84b78f77e39c9e0783f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-reliable-services"></a><span data-ttu-id="75546-103">Kom igång med Reliable Services</span><span class="sxs-lookup"><span data-stu-id="75546-103">Get started with Reliable Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="75546-104">C# i Windows</span><span class="sxs-lookup"><span data-stu-id="75546-104">C# on Windows</span></span>](service-fabric-reliable-services-quick-start.md)
> * [<span data-ttu-id="75546-105">Java i Linux</span><span class="sxs-lookup"><span data-stu-id="75546-105">Java on Linux</span></span>](service-fabric-reliable-services-quick-start-java.md)
> 
> 

<span data-ttu-id="75546-106">Ett Azure Service Fabric-program innehåller en eller flera tjänster som körs din kod.</span><span class="sxs-lookup"><span data-stu-id="75546-106">An Azure Service Fabric application contains one or more services that run your code.</span></span> <span data-ttu-id="75546-107">Den här guiden visar hur du skapar både tillståndslösa och tillståndskänsliga Service Fabric-program med [Reliable Services](service-fabric-reliable-services-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="75546-107">This guide shows you how to create both stateless and stateful Service Fabric applications with [Reliable Services](service-fabric-reliable-services-introduction.md).</span></span>  <span data-ttu-id="75546-108">Den här Microsoft Virtual Academy video visar även hur du skapar en tillståndslös tillförlitlig tjänst:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965"></span><span class="sxs-lookup"><span data-stu-id="75546-108">This Microsoft Virtual Academy video also shows you how to create a stateless Reliable service: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965"></span></span>  
<img src="./media/service-fabric-reliable-services-quick-start/ReliableServicesVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="basic-concepts"></a><span data-ttu-id="75546-109">Grundläggande begrepp</span><span class="sxs-lookup"><span data-stu-id="75546-109">Basic concepts</span></span>
<span data-ttu-id="75546-110">Om du vill komma igång med Reliable Services, behöver du bara förstå några grundläggande begrepp:</span><span class="sxs-lookup"><span data-stu-id="75546-110">To get started with Reliable Services, you only need to understand a few basic concepts:</span></span>

* <span data-ttu-id="75546-111">**Typen tjänst**: det här är din implementering.</span><span class="sxs-lookup"><span data-stu-id="75546-111">**Service type**: This is your service implementation.</span></span> <span data-ttu-id="75546-112">Den definieras av den klass som utökar du skriver `StatelessService` och annan kod eller beroenden, används tillsammans med ett namn och ett versionsnummer.</span><span class="sxs-lookup"><span data-stu-id="75546-112">It is defined by the class you write that extends `StatelessService` and any other code or dependencies used therein, along with a name and a version number.</span></span>
* <span data-ttu-id="75546-113">**Med namnet tjänstinstans**: för att köra din tjänst måste du skapa namngivna instanser av service-typen mycket som du skapar objektinstanser av en klasstyp.</span><span class="sxs-lookup"><span data-stu-id="75546-113">**Named service instance**: To run your service, you create named instances of your service type, much like you create object instances of a class type.</span></span> <span data-ttu-id="75546-114">En tjänstinstans har ett namn i form av en URI med hjälp av den ”fabric: /” schemat, till exempel ”fabric: / MyApp/MyService”.</span><span class="sxs-lookup"><span data-stu-id="75546-114">A service instance has a name in the form of a URI using the "fabric:/" scheme, such as "fabric:/MyApp/MyService".</span></span>
* <span data-ttu-id="75546-115">**Tjänstvärden**: namngiven tjänstinstanser som du skapar måste du köra inom en värdprocess.</span><span class="sxs-lookup"><span data-stu-id="75546-115">**Service host**: The named service instances you create need to run inside a host process.</span></span> <span data-ttu-id="75546-116">Tjänstevärden är bara en process där instanser av tjänsten kan köras.</span><span class="sxs-lookup"><span data-stu-id="75546-116">The service host is just a process where instances of your service can run.</span></span>
* <span data-ttu-id="75546-117">**Registrering av tjänst**: registrering sammanför allt.</span><span class="sxs-lookup"><span data-stu-id="75546-117">**Service registration**: Registration brings everything together.</span></span> <span data-ttu-id="75546-118">Tjänsttypen måste registreras med Service Fabric-körning i en tjänstevärd för att tillåta Service Fabric skapar instanser av den att köra.</span><span class="sxs-lookup"><span data-stu-id="75546-118">The service type must be registered with the Service Fabric runtime in a service host to allow Service Fabric to create instances of it to run.</span></span>  

## <a name="create-a-stateless-service"></a><span data-ttu-id="75546-119">Skapa en tillståndslös tjänst</span><span class="sxs-lookup"><span data-stu-id="75546-119">Create a stateless service</span></span>
<span data-ttu-id="75546-120">En tillståndslös tjänst är en typ av tjänst som används för närvarande normen i molnprogram.</span><span class="sxs-lookup"><span data-stu-id="75546-120">A stateless service is a type of service that is currently the norm in cloud applications.</span></span> <span data-ttu-id="75546-121">Anses tillståndslös eftersom själva tjänsten inte innehåller data som behöver lagras på ett tillförlitligt sätt eller hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="75546-121">It is considered stateless because the service itself does not contain data that needs to be stored reliably or made highly available.</span></span> <span data-ttu-id="75546-122">Om en instans av en tillståndslösa tjänsten stängs av, förloras alla det interna tillståndet.</span><span class="sxs-lookup"><span data-stu-id="75546-122">If an instance of a stateless service shuts down, all of its internal state is lost.</span></span> <span data-ttu-id="75546-123">I den här typen av tjänsten måste tillstånd sparas till en extern butik, till exempel Azure-tabeller eller en SQL-databas för att den ska göras hög tillgänglighet och tillförlitlig.</span><span class="sxs-lookup"><span data-stu-id="75546-123">In this type of service, state must be persisted to an external store, such as Azure Tables or a SQL database, for it to be made highly available and reliable.</span></span>

<span data-ttu-id="75546-124">Starta Visual Studio 2015 eller Visual Studio 2017 som administratör och skapa ett nytt Service Fabric application projekt med namnet *HelloWorld*:</span><span class="sxs-lookup"><span data-stu-id="75546-124">Launch Visual Studio 2015 or Visual Studio 2017 as an administrator, and create a new Service Fabric application project named *HelloWorld*:</span></span>

![Använd dialogrutan Nytt projekt för att skapa ett nytt Service Fabric-program](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject.png)

<span data-ttu-id="75546-126">Skapa ett projekt för tillståndslösa tjänsten med namnet *HelloWorldStateless*:</span><span class="sxs-lookup"><span data-stu-id="75546-126">Then create a stateless service project named *HelloWorldStateless*:</span></span>

![Skapa ett projekt för tillståndslösa tjänsten i den andra dialogrutan](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject2.png)

<span data-ttu-id="75546-128">Din lösning innehåller nu två projekt:</span><span class="sxs-lookup"><span data-stu-id="75546-128">Your solution now contains two projects:</span></span>

* <span data-ttu-id="75546-129">*HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="75546-129">*HelloWorld*.</span></span> <span data-ttu-id="75546-130">Det här är den *programmet* projekt som innehåller din *services*.</span><span class="sxs-lookup"><span data-stu-id="75546-130">This is the *application* project that contains your *services*.</span></span> <span data-ttu-id="75546-131">Den innehåller också programmanifestet som beskriver programmet, samt ett antal PowerShell-skript som hjälper dig att distribuera ditt program.</span><span class="sxs-lookup"><span data-stu-id="75546-131">It also contains the application manifest that describes the application, as well as a number of PowerShell scripts that help you to deploy your application.</span></span>
* <span data-ttu-id="75546-132">*HelloWorldStateless*.</span><span class="sxs-lookup"><span data-stu-id="75546-132">*HelloWorldStateless*.</span></span> <span data-ttu-id="75546-133">Detta är service-projekt.</span><span class="sxs-lookup"><span data-stu-id="75546-133">This is the service project.</span></span> <span data-ttu-id="75546-134">Den innehåller den tillståndslösa tjänstimplementeringen.</span><span class="sxs-lookup"><span data-stu-id="75546-134">It contains the stateless service implementation.</span></span>

## <a name="implement-the-service"></a><span data-ttu-id="75546-135">Implementera tjänsten</span><span class="sxs-lookup"><span data-stu-id="75546-135">Implement the service</span></span>
<span data-ttu-id="75546-136">Öppna den **HelloWorldStateless.cs** filen i service-projekt.</span><span class="sxs-lookup"><span data-stu-id="75546-136">Open the **HelloWorldStateless.cs** file in the service project.</span></span> <span data-ttu-id="75546-137">En tjänst kan köra all affärslogik i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="75546-137">In Service Fabric, a service can run any business logic.</span></span> <span data-ttu-id="75546-138">API för tjänsten tillhandahåller två startpunkter för din kod:</span><span class="sxs-lookup"><span data-stu-id="75546-138">The service API provides two entry points for your code:</span></span>

* <span data-ttu-id="75546-139">En öppen metoden, kallas *RunAsync*, där du kan börja köra alla arbetsbelastningar, inklusive tidskrävande beräkning av arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="75546-139">An open-ended entry point method, called *RunAsync*, where you can begin executing any workloads, including long-running compute workloads.</span></span>

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}
```

* <span data-ttu-id="75546-140">En kommunikation startpunkt där du kan ansluta din kommunikation stack föredrar, till exempel ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75546-140">A communication entry point where you can plug in your communication stack of choice, such as ASP.NET Core.</span></span> <span data-ttu-id="75546-141">Det är där du kan börja ta emot begäranden från användare och andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="75546-141">This is where you can start receiving requests from users and other services.</span></span>

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}
```

<span data-ttu-id="75546-142">I den här självstudiekursen kommer vi fokusera på den `RunAsync()` metoden.</span><span class="sxs-lookup"><span data-stu-id="75546-142">In this tutorial, we will focus on the `RunAsync()` entry point method.</span></span> <span data-ttu-id="75546-143">Det är där du kan starta direkt köra din kod.</span><span class="sxs-lookup"><span data-stu-id="75546-143">This is where you can immediately start running your code.</span></span>
<span data-ttu-id="75546-144">Projektmallen innehåller ett exempel på implementering av `RunAsync()` som ökar en löpande uppräkning.</span><span class="sxs-lookup"><span data-stu-id="75546-144">The project template includes a sample implementation of `RunAsync()` that increments a rolling count.</span></span>

> [!NOTE]
> <span data-ttu-id="75546-145">Mer information om hur du arbetar med en stack kommunikation finns [Service Fabric Web API-tjänster med OWIN själva värd](service-fabric-reliable-services-communication-webapi.md)</span><span class="sxs-lookup"><span data-stu-id="75546-145">For details about how to work with a communication stack, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md)</span></span>
> 
> 

### <a name="runasync"></a><span data-ttu-id="75546-146">RunAsync</span><span class="sxs-lookup"><span data-stu-id="75546-146">RunAsync</span></span>
```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following sample code with your own logic
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

<span data-ttu-id="75546-147">Plattformen som anropar den här metoden när en instans av en tjänst är monterade och redo att köra.</span><span class="sxs-lookup"><span data-stu-id="75546-147">The platform calls this method when an instance of a service is placed and ready to execute.</span></span> <span data-ttu-id="75546-148">För en tillståndslös tjänst innebär som bara när tjänstinstansen öppnas.</span><span class="sxs-lookup"><span data-stu-id="75546-148">For a stateless service, that simply means when the service instance is opened.</span></span> <span data-ttu-id="75546-149">En token för annullering tillhandahålls koordinera när service-instans måste stängas.</span><span class="sxs-lookup"><span data-stu-id="75546-149">A cancellation token is provided to coordinate when your service instance needs to be closed.</span></span> <span data-ttu-id="75546-150">I Service Fabric inträffa cykeln Öppna/stänga av en tjänstinstans många gånger under livslängden för tjänsten som helhet.</span><span class="sxs-lookup"><span data-stu-id="75546-150">In Service Fabric, this open/close cycle of a service instance can occur many times over the lifetime of the service as a whole.</span></span> <span data-ttu-id="75546-151">Detta kan inträffa av olika orsaker, bland annat:</span><span class="sxs-lookup"><span data-stu-id="75546-151">This can happen for various reasons, including:</span></span>

* <span data-ttu-id="75546-152">Systemet flyttar instanser av tjänsten för resurser.</span><span class="sxs-lookup"><span data-stu-id="75546-152">The system moves your service instances for resource balancing.</span></span>
* <span data-ttu-id="75546-153">Fel uppstår i koden.</span><span class="sxs-lookup"><span data-stu-id="75546-153">Faults occur in your code.</span></span>
* <span data-ttu-id="75546-154">Programmets eller systemets uppgraderas.</span><span class="sxs-lookup"><span data-stu-id="75546-154">The application or system is upgraded.</span></span>
* <span data-ttu-id="75546-155">Den underliggande maskinvaran skulle få ett avbrott.</span><span class="sxs-lookup"><span data-stu-id="75546-155">The underlying hardware experiences an outage.</span></span>

<span data-ttu-id="75546-156">Den här orchestration hanteras av systemet för att hålla din tjänst hög tillgänglighet och korrekt belastningsutjämnade.</span><span class="sxs-lookup"><span data-stu-id="75546-156">This orchestration is managed by the system to keep your service highly available and properly balanced.</span></span>

<span data-ttu-id="75546-157">`RunAsync()`bör inte blockera synkront.</span><span class="sxs-lookup"><span data-stu-id="75546-157">`RunAsync()` should not block synchronously.</span></span> <span data-ttu-id="75546-158">Implementeringen av RunAsync ska returnera en uppgift eller await på alla tidskrävande eller blockerar åtgärder för att fortsätta körningen.</span><span class="sxs-lookup"><span data-stu-id="75546-158">Your implementation of RunAsync should return a Task or await on any long-running or blocking operations to allow the runtime to continue.</span></span> <span data-ttu-id="75546-159">Observera i den `while(true)` loop i föregående exempel är en åtgärdsreturnerande `await Task.Delay()` används.</span><span class="sxs-lookup"><span data-stu-id="75546-159">Note in the `while(true)` loop in the previous example, a Task-returning `await Task.Delay()` is used.</span></span> <span data-ttu-id="75546-160">Om din arbetsbelastning måste blockerar synkront kan du schemalägga en ny uppgift med `Task.Run()` i din `RunAsync` implementering.</span><span class="sxs-lookup"><span data-stu-id="75546-160">If your workload must block synchronously, you should schedule a new Task with `Task.Run()` in your `RunAsync` implementation.</span></span>

<span data-ttu-id="75546-161">Annullering av din arbetsbelastning är en samverkande ansträngning styrd av den angivna annullering-token.</span><span class="sxs-lookup"><span data-stu-id="75546-161">Cancellation of your workload is a cooperative effort orchestrated by the provided cancellation token.</span></span> <span data-ttu-id="75546-162">Systemet väntar på uppgiften till slutet (genom lyckades, avbokning eller fel) innan den flyttas på.</span><span class="sxs-lookup"><span data-stu-id="75546-162">The system will wait for your task to end (by successful completion, cancellation, or fault) before it moves on.</span></span> <span data-ttu-id="75546-163">Det är viktigt att respektera cancellation-token, Slutför allt arbete och avsluta `RunAsync()` så snabbt som möjligt när systemet begär annullering.</span><span class="sxs-lookup"><span data-stu-id="75546-163">It is important to honor the cancellation token, finish any work, and exit `RunAsync()` as quickly as possible when the system requests cancellation.</span></span>

<span data-ttu-id="75546-164">I det här exemplet tillståndslösa tjänsten lagras antalet i en lokal variabel.</span><span class="sxs-lookup"><span data-stu-id="75546-164">In this stateless service example, the count is stored in a local variable.</span></span> <span data-ttu-id="75546-165">Men eftersom detta är en tillståndslös tjänst är det värde som lagras finns bara för den aktuella livscykeln för dess tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="75546-165">But because this is a stateless service, the value that's stored exists only for the current lifecycle of its service instance.</span></span> <span data-ttu-id="75546-166">När tjänsten startas om flyttar går värdet förlorad.</span><span class="sxs-lookup"><span data-stu-id="75546-166">When the service moves or restarts, the value is lost.</span></span>

## <a name="create-a-stateful-service"></a><span data-ttu-id="75546-167">Skapa en tillståndskänslig tjänst</span><span class="sxs-lookup"><span data-stu-id="75546-167">Create a stateful service</span></span>
<span data-ttu-id="75546-168">Service Fabric introducerar en ny typ av tjänst som är tillståndskänslig.</span><span class="sxs-lookup"><span data-stu-id="75546-168">Service Fabric introduces a new kind of service that is stateful.</span></span> <span data-ttu-id="75546-169">En tillståndskänslig service kan upprätthålla tillstånd på ett tillförlitligt sätt inom tjänsten samordnad med kod som använder den.</span><span class="sxs-lookup"><span data-stu-id="75546-169">A stateful service can maintain state reliably within the service itself, co-located with the code that's using it.</span></span> <span data-ttu-id="75546-170">Tillstånd görs högtillgänglig av Service Fabric utan att behöva spara tillstånd för att en extern butik.</span><span class="sxs-lookup"><span data-stu-id="75546-170">State is made highly available by Service Fabric without the need to persist state to an external store.</span></span>

<span data-ttu-id="75546-171">Om du vill konvertera ett värde för prestandaräknaren från tillståndslös till hög tillgänglighet och beständiga, även när tjänsten flyttas eller startas om, behöver du en tillståndskänslig tjänst.</span><span class="sxs-lookup"><span data-stu-id="75546-171">To convert a counter value from stateless to highly available and persistent, even when the service moves or restarts, you need a stateful service.</span></span>

<span data-ttu-id="75546-172">I samma *HelloWorld* programmet, du kan lägga till en ny tjänst genom att högerklicka på tjänsterna referenser i projektet för konsolprogrammet och välja **Lägg till ny tjänst för Service Fabric ->**.</span><span class="sxs-lookup"><span data-stu-id="75546-172">In the same *HelloWorld* application, you can add a new service by right-clicking on the Services references in the application project and selecting **Add ->  New Service Fabric Service**.</span></span>

![Lägga till en tjänst till Service Fabric-program](media/service-fabric-reliable-services-quick-start/hello-stateful-NewService.png)

<span data-ttu-id="75546-174">Välj **tillståndskänslig Service** och ger den namnet *HelloWorldStateful*.</span><span class="sxs-lookup"><span data-stu-id="75546-174">Select **Stateful Service** and name it *HelloWorldStateful*.</span></span> <span data-ttu-id="75546-175">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="75546-175">Click **OK**.</span></span>

![Använd dialogrutan Nytt projekt för att skapa en ny tillståndskänslig Service Fabric-tjänst](media/service-fabric-reliable-services-quick-start/hello-stateful-NewProject.png)

<span data-ttu-id="75546-177">Ditt program bör nu ha två tjänster: tillståndslösa tjänsten *HelloWorldStateless* och tillståndskänsliga tjänsten *HelloWorldStateful*.</span><span class="sxs-lookup"><span data-stu-id="75546-177">Your application should now have two services: the stateless service *HelloWorldStateless* and the stateful service *HelloWorldStateful*.</span></span>

<span data-ttu-id="75546-178">En tillståndskänslig tjänst har samma startpunkter som en tillståndslös tjänst.</span><span class="sxs-lookup"><span data-stu-id="75546-178">A stateful service has the same entry points as a stateless service.</span></span> <span data-ttu-id="75546-179">Den största skillnaden är tillgängligheten för en *tillståndsprovidern* som kan lagra tillstånd på ett tillförlitligt sätt.</span><span class="sxs-lookup"><span data-stu-id="75546-179">The main difference is the availability of a *state provider* that can store state reliably.</span></span> <span data-ttu-id="75546-180">Service Fabric medföljer en implementering av provider för tillstånd kallas [tillförlitliga samlingar](service-fabric-reliable-services-reliable-collections.md), vilket låter dig skapa replikeras datastrukturer via Hanteraren för tillförlitlig tillstånd.</span><span class="sxs-lookup"><span data-stu-id="75546-180">Service Fabric comes with a state provider implementation called [Reliable Collections](service-fabric-reliable-services-reliable-collections.md), which lets you create replicated data structures through the Reliable State Manager.</span></span> <span data-ttu-id="75546-181">En tillståndskänslig tillförlitlig tjänst använder den här tillståndsprovidern som standard.</span><span class="sxs-lookup"><span data-stu-id="75546-181">A stateful Reliable Service uses this state provider by default.</span></span>

<span data-ttu-id="75546-182">Öppna **HelloWorldStateful.cs** i *HelloWorldStateful*, som innehåller följande RunAsync metod:</span><span class="sxs-lookup"><span data-stu-id="75546-182">Open **HelloWorldStateful.cs** in *HelloWorldStateful*, which contains the following RunAsync method:</span></span>

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following sample code with your own logic
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

            // If an exception is thrown before calling CommitAsync, the transaction aborts, all changes are
            // discarded, and nothing is saved to the secondary replicas.
            await tx.CommitAsync();
        }

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
```

### <a name="runasync"></a><span data-ttu-id="75546-183">RunAsync</span><span class="sxs-lookup"><span data-stu-id="75546-183">RunAsync</span></span>
<span data-ttu-id="75546-184">`RunAsync()`fungerar på samma sätt i tillståndskänsliga och tillståndslösa tjänster.</span><span class="sxs-lookup"><span data-stu-id="75546-184">`RunAsync()` operates similarly in stateful and stateless services.</span></span> <span data-ttu-id="75546-185">Dock i en tillståndskänslig service plattformen utför ytterligare arbete för din räkning innan den kör `RunAsync()`.</span><span class="sxs-lookup"><span data-stu-id="75546-185">However, in a stateful service, the platform performs additional work on your behalf before it executes `RunAsync()`.</span></span> <span data-ttu-id="75546-186">Detta verk kan inkludera att säkerställa att tillförlitliga Tillståndshanterare och tillförlitlig samlingar är redo att användas.</span><span class="sxs-lookup"><span data-stu-id="75546-186">This work can include ensuring that the Reliable State Manager and Reliable Collections are ready to use.</span></span>

### <a name="reliable-collections-and-the-reliable-state-manager"></a><span data-ttu-id="75546-187">Tillförlitliga samlingar och hanteraren för tillförlitlig tillstånd</span><span class="sxs-lookup"><span data-stu-id="75546-187">Reliable Collections and the Reliable State Manager</span></span>
```csharp
var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
```

<span data-ttu-id="75546-188">[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) är en dictionary-implementering som du kan använda för att lagra på ett tillförlitligt sätt tillstånd i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="75546-188">[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) is a dictionary implementation that you can use to reliably store state in the service.</span></span> <span data-ttu-id="75546-189">Med Service Fabric och tillförlitlig samlingar, kan du lagra data direkt i din tjänst utan behov av en extern beständiga arkivet.</span><span class="sxs-lookup"><span data-stu-id="75546-189">With Service Fabric and Reliable Collections, you can store data directly in your service without the need for an external persistent store.</span></span> <span data-ttu-id="75546-190">Tillförlitliga samlingar gör att dina data är hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="75546-190">Reliable Collections make your data highly available.</span></span> <span data-ttu-id="75546-191">Service Fabric åstadkommer detta genom att skapa och hantera flera *repliker* av tjänsten för dig.</span><span class="sxs-lookup"><span data-stu-id="75546-191">Service Fabric accomplishes this by creating and managing multiple *replicas* of your service for you.</span></span> <span data-ttu-id="75546-192">Det ger också en API som avlägsnar direkt svårigheter för att hantera dessa repliker och deras tillståndsövergångar.</span><span class="sxs-lookup"><span data-stu-id="75546-192">It also provides an API that abstracts away the complexities of managing those replicas and their state transitions.</span></span>

<span data-ttu-id="75546-193">Tillförlitliga samlingar kan lagra alla .NET-typ, inklusive din anpassade typer med några varningar.</span><span class="sxs-lookup"><span data-stu-id="75546-193">Reliable Collections can store any .NET type, including your custom types, with a couple of caveats:</span></span>

* <span data-ttu-id="75546-194">Service Fabric gör ditt tillstånd hög tillgänglighet av *replikerar* tillstånd mellan noder och tillförlitlig samlingar lagra data på en lokal disk på varje replik.</span><span class="sxs-lookup"><span data-stu-id="75546-194">Service Fabric makes your state highly available by *replicating* state across nodes, and Reliable Collections store your data to local disk on each replica.</span></span> <span data-ttu-id="75546-195">Det innebär att allt som lagras i tillförlitliga samlingar måste vara *serialiserbara*.</span><span class="sxs-lookup"><span data-stu-id="75546-195">This means that everything that is stored in Reliable Collections must be *serializable*.</span></span> <span data-ttu-id="75546-196">Som standard använder tillförlitliga samlingar [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) för serialisering, så det är viktigt att se till att din typer är [stöds av datakontraktserialiserren](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) när du använder standard-serialisering.</span><span class="sxs-lookup"><span data-stu-id="75546-196">By default, Reliable Collections use [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) for serialization, so it's important to make sure that your types are [supported by the Data Contract Serializer](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) when you use the default serializer.</span></span>
* <span data-ttu-id="75546-197">Objekt replikeras för hög tillgänglighet när du gör transaktioner på tillförlitliga samlingar.</span><span class="sxs-lookup"><span data-stu-id="75546-197">Objects are replicated for high availability when you commit transactions on Reliable Collections.</span></span> <span data-ttu-id="75546-198">Objekt som lagras i tillförlitliga samlingar sparas i lokalt minne i din tjänst.</span><span class="sxs-lookup"><span data-stu-id="75546-198">Objects stored in Reliable Collections are kept in local memory in your service.</span></span> <span data-ttu-id="75546-199">Det innebär att du har en lokal referens till objektet.</span><span class="sxs-lookup"><span data-stu-id="75546-199">This means that you have a local reference to the object.</span></span>
  
   <span data-ttu-id="75546-200">Det är viktigt att du inte mutera lokala instanser av dessa objekt utan att utföra en Uppdateringsåtgärd i den tillförlitliga samlingen i en transaktion.</span><span class="sxs-lookup"><span data-stu-id="75546-200">It is important that you do not mutate local instances of those objects without performing an update operation on the reliable collection in a transaction.</span></span> <span data-ttu-id="75546-201">Det beror på att ändringar i lokala instanser av objekt inte replikeras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="75546-201">This is because changes to local instances of objects will not be replicated automatically.</span></span> <span data-ttu-id="75546-202">Du måste infoga objektet tillbaka till ordlistan igen eller Använd en av de *uppdatera* metoder i ordlistan.</span><span class="sxs-lookup"><span data-stu-id="75546-202">You must re-insert the object back into the dictionary or use one of the *update* methods on the dictionary.</span></span>

<span data-ttu-id="75546-203">Tillförlitliga Tillståndshanterarens hanterar tillförlitliga samlingar åt dig.</span><span class="sxs-lookup"><span data-stu-id="75546-203">The Reliable State Manager manages Reliable Collections for you.</span></span> <span data-ttu-id="75546-204">Du kan bara be tillförlitliga Tillståndshanterarens för en tillförlitlig samling med namnet när som helst och var som helst i din tjänst.</span><span class="sxs-lookup"><span data-stu-id="75546-204">You can simply ask the Reliable State Manager for a reliable collection by name at any time and at any place in your service.</span></span> <span data-ttu-id="75546-205">Den tillförlitliga tillstånd Manager ser till att du får en referens tillbaka.</span><span class="sxs-lookup"><span data-stu-id="75546-205">The Reliable State Manager ensures that you get a reference back.</span></span> <span data-ttu-id="75546-206">Inte rekommenderar vi att du sparar referenser till tillförlitliga samling instanser i klassmedlem egenskaper eller variabler.</span><span class="sxs-lookup"><span data-stu-id="75546-206">We don't recommended that you save references to reliable collection instances in class member variables or properties.</span></span> <span data-ttu-id="75546-207">Särskild försiktighet måste vidtas för att säkerställa att referensen har angetts till en instans alltid i livscykeln för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="75546-207">Special care must be taken to ensure that the reference is set to an instance at all times in the service lifecycle.</span></span> <span data-ttu-id="75546-208">Tillförlitliga Tillståndshanterarens hanterar det här fungerar och har optimerats för Upprepa besök.</span><span class="sxs-lookup"><span data-stu-id="75546-208">The Reliable State Manager handles this work for you, and it's optimized for repeat visits.</span></span>

### <a name="transactional-and-asynchronous-operations"></a><span data-ttu-id="75546-209">Transaktionell och asynkrona åtgärder</span><span class="sxs-lookup"><span data-stu-id="75546-209">Transactional and asynchronous operations</span></span>
```C#
using (ITransaction tx = this.StateManager.CreateTransaction())
{
    var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");

    await myDictionary.AddOrUpdateAsync(tx, "Counter-1", 0, (k, v) => ++v);

    await tx.CommitAsync();
}
```

<span data-ttu-id="75546-210">Tillförlitliga samlingar har många av åtgärderna som deras `System.Collections.Generic` och `System.Collections.Concurrent` motsvarigheter göra, förutom LINQ.</span><span class="sxs-lookup"><span data-stu-id="75546-210">Reliable Collections have many of the same operations that their `System.Collections.Generic` and `System.Collections.Concurrent` counterparts do, except LINQ.</span></span> <span data-ttu-id="75546-211">Åtgärder på tillförlitliga samlingar är asynkron.</span><span class="sxs-lookup"><span data-stu-id="75546-211">Operations on Reliable Collections are asynchronous.</span></span> <span data-ttu-id="75546-212">Det beror på att skrivåtgärder med tillförlitlig samlingar utföra i/o-åtgärder för att replikera och spara data till disk.</span><span class="sxs-lookup"><span data-stu-id="75546-212">This is because write operations with Reliable Collections perform I/O operations to replicate and persist data to disk.</span></span>

<span data-ttu-id="75546-213">Åtgärder för insamling av tillförlitliga är *transaktionella*, så att du kan behålla tillstånd konsekvent över flera tillförlitliga samlingar och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="75546-213">Reliable Collection operations are *transactional*, so that you can keep state consistent across multiple Reliable Collections and operations.</span></span> <span data-ttu-id="75546-214">Du kan till exempel ett arbetsobjekt från en tillförlitlig kö har status Created, utföra en åtgärd på det och spara resultatet i en tillförlitlig ordlista alla inom en enskild transaktion.</span><span class="sxs-lookup"><span data-stu-id="75546-214">For example, you may dequeue a work item from a Reliable Queue, perform an operation on it, and save the result in a Reliable Dictionary, all within a single transaction.</span></span> <span data-ttu-id="75546-215">Detta behandlas som en atomisk åtgärd och det garanterar att hela åtgärden lyckas eller hela åtgärden återställs.</span><span class="sxs-lookup"><span data-stu-id="75546-215">This is treated as an atomic operation, and it guarantees that either the entire operation will succeed or the entire operation will roll back.</span></span> <span data-ttu-id="75546-216">Om ett fel inträffar efter att objektet har status Created men innan du sparar resultatet hela transaktionen återställs och artikeln finns kvar i kön för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="75546-216">If an error occurs after you dequeue the item but before you save the result, the entire transaction is rolled back and the item remains in the queue for processing.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="75546-217">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="75546-217">Run the application</span></span>
<span data-ttu-id="75546-218">Vi Gå nu tillbaka till den *HelloWorld* program.</span><span class="sxs-lookup"><span data-stu-id="75546-218">We now return to the *HelloWorld* application.</span></span> <span data-ttu-id="75546-219">Du kan nu skapa och distribuera tjänster.</span><span class="sxs-lookup"><span data-stu-id="75546-219">You can now build and deploy your services.</span></span> <span data-ttu-id="75546-220">När du trycker på **F5**, ditt program kommer skapats och distribuerats till det lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="75546-220">When you press **F5**, your application will be built and deployed to your local cluster.</span></span>

<span data-ttu-id="75546-221">När tjänsten startas, kan du visa de genererade ETW Event Tracing for Windows ()-händelserna i en **diagnostiska händelser** fönster.</span><span class="sxs-lookup"><span data-stu-id="75546-221">After the services start running, you can view the generated Event Tracing for Windows (ETW) events in a **Diagnostic Events** window.</span></span> <span data-ttu-id="75546-222">Observera att de händelser som visas är från både tjänsten tillståndslösa och tillståndskänsliga tjänsten i programmet.</span><span class="sxs-lookup"><span data-stu-id="75546-222">Note that the events displayed are from both the stateless service and the stateful service in the application.</span></span> <span data-ttu-id="75546-223">Du kan pausa dataströmmen genom att klicka på den **pausa** knappen.</span><span class="sxs-lookup"><span data-stu-id="75546-223">You can pause the stream by clicking the **Pause** button.</span></span> <span data-ttu-id="75546-224">Du kan granska information om ett meddelande genom att expandera meddelandet.</span><span class="sxs-lookup"><span data-stu-id="75546-224">You can then examine the details of a message by expanding that message.</span></span>

> [!NOTE]
> <span data-ttu-id="75546-225">Kontrollera att du har en lokal utveckling kluster som kör innan du kör programmet.</span><span class="sxs-lookup"><span data-stu-id="75546-225">Before you run the application, make sure that you have a local development cluster running.</span></span> <span data-ttu-id="75546-226">Kolla in den [Kom igång med](service-fabric-get-started.md) information om hur du konfigurerar din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="75546-226">Check out the [getting started guide](service-fabric-get-started.md) for information on setting up your local environment.</span></span>
> 
> 

![Visa diagnostik händelser i Visual Studio](media/service-fabric-reliable-services-quick-start/hello-stateful-Output.png)

## <a name="next-steps"></a><span data-ttu-id="75546-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75546-228">Next steps</span></span>
[<span data-ttu-id="75546-229">Felsöka Service Fabric-program i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="75546-229">Debug your Service Fabric application in Visual Studio</span></span>](service-fabric-debugging-your-application.md)

[<span data-ttu-id="75546-230">Komma igång: Service Fabric Web API-tjänster med OWIN själva värd</span><span class="sxs-lookup"><span data-stu-id="75546-230">Get started: Service Fabric Web API services with OWIN self-hosting</span></span>](service-fabric-reliable-services-communication-webapi.md)

[<span data-ttu-id="75546-231">Mer information om tillförlitlig samlingar</span><span class="sxs-lookup"><span data-stu-id="75546-231">Learn more about Reliable Collections</span></span>](service-fabric-reliable-services-reliable-collections.md)

[<span data-ttu-id="75546-232">Distribuera ett program</span><span class="sxs-lookup"><span data-stu-id="75546-232">Deploy an application</span></span>](service-fabric-deploy-remove-applications.md)

[<span data-ttu-id="75546-233">Uppgradering av programmet</span><span class="sxs-lookup"><span data-stu-id="75546-233">Application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="75546-234">För utvecklare för Reliable Services</span><span class="sxs-lookup"><span data-stu-id="75546-234">Developer reference for Reliable Services</span></span>](https://msdn.microsoft.com/library/azure/dn706529.aspx)

