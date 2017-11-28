---
title: "aaaCreate din första aktören-baserad Azure mikrotjänster i C# | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom hello stegen för att skapa, felsöka och distribuera en enkel aktören-baserad tjänst med hjälp av Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d4aebe72-1551-4062-b1eb-54d83297f139
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: ab4f75bef0adb6e70f0ead587475b3fb51e6e6a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-reliable-actors"></a><span data-ttu-id="7c3d4-103">Komma igång med Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="7c3d4-103">Getting started with Reliable Actors</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7c3d4-104">C# i Windows</span><span class="sxs-lookup"><span data-stu-id="7c3d4-104">C# on Windows</span></span>](service-fabric-reliable-actors-get-started.md)
> * [<span data-ttu-id="7c3d4-105">Java i Linux</span><span class="sxs-lookup"><span data-stu-id="7c3d4-105">Java on Linux</span></span>](service-fabric-reliable-actors-get-started-java.md)
> 
> 

<span data-ttu-id="7c3d4-106">Den här artikeln beskriver hello grunderna i Azure Service Fabric Reliable Actors och vägleder dig genom att skapa, felsöka och distribuera ett enkelt tillförlitliga aktören program i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-106">This article explains hello basics of Azure Service Fabric Reliable Actors and walks you through creating, debugging, and deploying a simple Reliable Actor application in Visual Studio.</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="7c3d4-107">Installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="7c3d4-107">Installation and setup</span></span>
<span data-ttu-id="7c3d4-108">Innan du börjar bör du kontrollera att du har hello Service Fabric-utvecklingsmiljö ställa in på din dator.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-108">Before you start, ensure that you have hello Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="7c3d4-109">Om du behöver tooset den, se detaljerade anvisningar på [hur tooset hello utvecklingsmiljön](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7c3d4-109">If you need tooset it up, see detailed instructions on [how tooset up hello development environment](service-fabric-get-started.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="7c3d4-110">Grundläggande begrepp</span><span class="sxs-lookup"><span data-stu-id="7c3d4-110">Basic concepts</span></span>
<span data-ttu-id="7c3d4-111">tooget igång med Reliable Actors du bara behöver toounderstand några grundläggande begrepp:</span><span class="sxs-lookup"><span data-stu-id="7c3d4-111">tooget started with Reliable Actors, you only need toounderstand a few basic concepts:</span></span>

* <span data-ttu-id="7c3d4-112">**Aktören tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-112">**Actor service**.</span></span> <span data-ttu-id="7c3d4-113">Reliable Actors paketeras i Reliable Services som kan distribueras i hello Service Fabric-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-113">Reliable Actors are packaged in Reliable Services that can be deployed in hello Service Fabric infrastructure.</span></span> <span data-ttu-id="7c3d4-114">Aktören instanser aktiveras på en namngiven tjänstinstans.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-114">Actor instances are activated in a named service instance.</span></span>
* <span data-ttu-id="7c3d4-115">**Aktören registrering**.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-115">**Actor registration**.</span></span> <span data-ttu-id="7c3d4-116">Som med Reliable Services en tillförlitlig aktören tjänst behöver toobe som registrerats med hello Service Fabric runtime.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-116">As with Reliable Services, a Reliable Actor service needs toobe registered with hello Service Fabric runtime.</span></span> <span data-ttu-id="7c3d4-117">Hello aktörstyp måste dessutom toobe som registrerats med hello aktören körning.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-117">In addition, hello actor type needs toobe registered with hello Actor runtime.</span></span>
* <span data-ttu-id="7c3d4-118">**Aktören gränssnittet**.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-118">**Actor interface**.</span></span> <span data-ttu-id="7c3d4-119">hello aktören gränssnittet är används toodefine ett starkt typifierad offentliga gränssnitt för en aktör.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-119">hello actor interface is used toodefine a strongly typed public interface of an actor.</span></span> <span data-ttu-id="7c3d4-120">I hello tillförlitliga aktören modellen terminologi definierar hello aktören-gränssnittet hello typer av meddelanden som hello aktören kan förstå och bearbeta.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-120">In hello Reliable Actor model terminology, hello actor interface defines hello types of messages that hello actor can understand and process.</span></span> <span data-ttu-id="7c3d4-121">hello aktören gränssnitt som används av andra aktörer och klientprogram för ”skicka” (asynkront) meddelanden toohello aktören.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-121">hello actor interface is used by other actors and client applications too"send" (asynchronously) messages toohello actor.</span></span> <span data-ttu-id="7c3d4-122">Reliable Actors kan implementera flera gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-122">Reliable Actors can implement multiple interfaces.</span></span>
* <span data-ttu-id="7c3d4-123">**ActorProxy klassen**.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-123">**ActorProxy class**.</span></span> <span data-ttu-id="7c3d4-124">Hej ActorProxy klass som används av klienten program tooinvoke hello metoder som exponeras via hello aktören gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-124">hello ActorProxy class is used by client applications tooinvoke hello methods exposed through hello actor interface.</span></span> <span data-ttu-id="7c3d4-125">Hej ActorProxy klassen innehåller två viktiga funktioner:</span><span class="sxs-lookup"><span data-stu-id="7c3d4-125">hello ActorProxy class provides two important functionalities:</span></span>
  
  * <span data-ttu-id="7c3d4-126">Namnmatchning: det är kan toolocate hello aktören i hello kluster (Sök hello-nod i hello kluster där den finns).</span><span class="sxs-lookup"><span data-stu-id="7c3d4-126">Name resolution: It is able toolocate hello actor in hello cluster (find hello node of hello cluster where it is hosted).</span></span>
  * <span data-ttu-id="7c3d4-127">Hantering av fel: den gör anrop av metoden och nytt lösa hello aktören platsen efter, t.ex, ett fel som kräver hello aktören toobe flyttas tooanother nod i klustret för hello.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-127">Failure handling: It can retry method invocations and re-resolve hello actor location after, for example, a failure that requires hello actor toobe relocated tooanother node in hello cluster.</span></span>

<span data-ttu-id="7c3d4-128">hello enligt reglerna som gäller tooactor gränssnitt är värt att nämna:</span><span class="sxs-lookup"><span data-stu-id="7c3d4-128">hello following rules that pertain tooactor interfaces are worth mentioning:</span></span>

* <span data-ttu-id="7c3d4-129">Aktören gränssnittsmetoder kan vara överbelastad.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-129">Actor interface methods cannot be overloaded.</span></span>
* <span data-ttu-id="7c3d4-130">Aktören gränssnittet metoder inte får ha ut, ref eller valfria parametrar.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-130">Actor interface methods must not have out, ref, or optional parameters.</span></span>
* <span data-ttu-id="7c3d4-131">Allmänt gränssnitt stöds inte.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-131">Generic interfaces are not supported.</span></span>

## <a name="create-a-new-project-in-visual-studio"></a><span data-ttu-id="7c3d4-132">Skapa ett nytt projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c3d4-132">Create a new project in Visual Studio</span></span>
<span data-ttu-id="7c3d4-133">Starta Visual Studio 2015 eller Visual Studio 2017 som administratör och skapa ett nytt projekt för Service Fabric-program:</span><span class="sxs-lookup"><span data-stu-id="7c3d4-133">Launch Visual Studio 2015 or Visual Studio 2017 as an administrator, and create a new Service Fabric application project:</span></span>

![Service Fabric-verktyg för Visual Studio – nytt projekt][1]

<span data-ttu-id="7c3d4-135">Hello nästa i dialogrutan kan du välja hello typ av projekt som du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-135">In hello next dialog box, you can choose hello type of project that you want toocreate.</span></span>

![Service Fabric-projektmallar][5]

<span data-ttu-id="7c3d4-137">För hello HelloWorld projektet ska vi använda hello Reliable Actors för Service Fabric-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-137">For hello HelloWorld project, let's use hello Service Fabric Reliable Actors service.</span></span>

<span data-ttu-id="7c3d4-138">När du har skapat hello-lösning bör du se hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="7c3d4-138">After you have created hello solution, you should see hello following structure:</span></span>

![Struktur för Service Fabric-projekt][2]

## <a name="reliable-actors-basic-building-blocks"></a><span data-ttu-id="7c3d4-140">Tillförlitliga aktörer grundläggande byggstenarna</span><span class="sxs-lookup"><span data-stu-id="7c3d4-140">Reliable Actors basic building blocks</span></span>
<span data-ttu-id="7c3d4-141">En typisk Reliable Actors lösning består av tre projekt:</span><span class="sxs-lookup"><span data-stu-id="7c3d4-141">A typical Reliable Actors solution is composed of three projects:</span></span>

* <span data-ttu-id="7c3d4-142">**hello projektet (MyActorApplication)**.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-142">**hello application project (MyActorApplication)**.</span></span> <span data-ttu-id="7c3d4-143">Detta är hello-projekt som paket alla hello tillsammans för distribution.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-143">This is hello project that packages all of hello services together for deployment.</span></span> <span data-ttu-id="7c3d4-144">Den innehåller hello *ApplicationManifest.xml* och PowerShell-skript för att hantera hello program.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-144">It contains hello *ApplicationManifest.xml* and PowerShell scripts for managing hello application.</span></span>
* <span data-ttu-id="7c3d4-145">**hello gränssnittet projekt (MyActor.Interfaces)**.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-145">**hello interface project (MyActor.Interfaces)**.</span></span> <span data-ttu-id="7c3d4-146">Detta är hello-projekt som innehåller hello gränssnittsdefinition för hello aktören.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-146">This is hello project that contains hello interface definition for hello actor.</span></span> <span data-ttu-id="7c3d4-147">Du kan definiera hello-gränssnitt som ska användas av hello aktörer i hello lösning i hello MyActor.Interfaces-projektet.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-147">In hello MyActor.Interfaces project, you can define hello interfaces that will be used by hello actors in hello solution.</span></span> <span data-ttu-id="7c3d4-148">Gränssnitten aktören kan definieras i ett projekt med ett namn, men hello-gränssnittet definierar hello aktören kontrakt som delas av hello aktören implementering och hello klienter anropar hello aktören, så gör det vanligtvis meningsfullt toodefine den i en sammansättning som är separata från hello aktören implementering och kan delas av flera projekt.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-148">Your actor interfaces can be defined in any project with any name, however hello interface defines hello actor contract that is shared by hello actor implementation and hello clients calling hello actor, so it typically makes sense toodefine it in an assembly that is separate from hello actor implementation and can be shared by multiple other projects.</span></span>

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* <span data-ttu-id="7c3d4-149">**hello aktören service-projekt (MyActor)**.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-149">**hello actor service project (MyActor)**.</span></span> <span data-ttu-id="7c3d4-150">Detta är hello projekt används toodefine hello Service Fabric-tjänsten som är pågående toohost hello aktören.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-150">This is hello project used toodefine hello Service Fabric service that is going toohost hello actor.</span></span> <span data-ttu-id="7c3d4-151">Den innehåller hello implementering av hello aktören.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-151">It contains hello implementation of hello actor.</span></span> <span data-ttu-id="7c3d4-152">En aktören implementering är en klass som härleds från hello bastypen `Actor` och implementerar hello gränssnitt som är definierade i hello MyActor.Interfaces projekt.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-152">An actor implementation is a class that derives from hello base type `Actor` and implements hello interface(s) that are defined in hello MyActor.Interfaces project.</span></span> <span data-ttu-id="7c3d4-153">En aktörklass måste också implementera en konstruktor som accepterar ett `ActorService` instans och en `ActorId` och skickar dem toohello grundläggande `Actor` klass.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-153">An actor class must also implement a constructor that accepts an `ActorService` instance and an `ActorId` and passes them toohello base `Actor` class.</span></span> <span data-ttu-id="7c3d4-154">Detta ger konstruktorn beroendeinmatning plattform beroenden.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-154">This allows for constructor dependency injection of platform dependencies.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

<span data-ttu-id="7c3d4-155">hello aktören service måste ha registrerats med en typ i hello Service Fabric-körningsmiljön.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-155">hello actor service must be registered with a service type in hello Service Fabric runtime.</span></span> <span data-ttu-id="7c3d4-156">I ordning för hello aktören Service toorun aktören-instanser, aktören-typen måste också vara registrerad med hello aktören Service.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-156">In order for hello Actor Service toorun your actor instances, your actor type must also be registered with hello Actor Service.</span></span> <span data-ttu-id="7c3d4-157">Hej `ActorRuntime` registreringsmetod utför detta arbete för aktörer.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-157">hello `ActorRuntime` registration method performs this work for actors.</span></span>

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

<span data-ttu-id="7c3d4-158">Om du startar från ett nytt projekt i Visual Studio och du bara ha en aktören definition, med hello registrering som standard i hello-kod som genereras av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-158">If you start from a new project in Visual Studio and you have only one actor definition, hello registration is included by default in hello code that Visual Studio generates.</span></span> <span data-ttu-id="7c3d4-159">Om du definierar andra aktörer i hello tjänsten behöver tooadd hello aktören registrering genom att använda:</span><span class="sxs-lookup"><span data-stu-id="7c3d4-159">If you define other actors in hello service, you need tooadd hello actor registration by using:</span></span>

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [!TIP]
> <span data-ttu-id="7c3d4-160">hello Service Fabric aktörer runtime skickar vissa [händelser och Prestandaräknare relaterade tooactor metoder](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters).</span><span class="sxs-lookup"><span data-stu-id="7c3d4-160">hello Service Fabric Actors runtime emits some [events and performance counters related tooactor methods](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters).</span></span> <span data-ttu-id="7c3d4-161">De är användbara i diagnostik- och prestandaövervakning.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-161">They are useful in diagnostics and performance monitoring.</span></span>
> 
> 

## <a name="debugging"></a><span data-ttu-id="7c3d4-162">Felsökning</span><span class="sxs-lookup"><span data-stu-id="7c3d4-162">Debugging</span></span>
<span data-ttu-id="7c3d4-163">hello Service Fabric-verktyg för Visual Studio stöder felsökning på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-163">hello Service Fabric tools for Visual Studio support debugging on your local machine.</span></span> <span data-ttu-id="7c3d4-164">Du kan starta en felsökning av hitting hello F5-tangenten.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-164">You can start a debugging session by hitting hello F5 key.</span></span> <span data-ttu-id="7c3d4-165">Visual Studio skapar (vid behov) paket.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-165">Visual Studio builds (if necessary) packages.</span></span> <span data-ttu-id="7c3d4-166">Den distribuerar programmet hello på hello lokala Service Fabric-kluster och bifogar hello felsökare.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-166">It also deploys hello application on hello local Service Fabric cluster and attaches hello debugger.</span></span>

<span data-ttu-id="7c3d4-167">Under processen för distribution av hello ser du hello förlopp i hello **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="7c3d4-167">During hello deployment process, you can see hello progress in hello **Output** window.</span></span>

![Service Fabric felsökning utdatafönstret][3]

## <a name="next-steps"></a><span data-ttu-id="7c3d4-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c3d4-169">Next steps</span></span>
<span data-ttu-id="7c3d4-170">Lär dig mer om [hur Reliable Actors använda hello Service Fabric-plattformen](service-fabric-reliable-actors-platform.md).</span><span class="sxs-lookup"><span data-stu-id="7c3d4-170">Learn more about [how Reliable Actors use hello Service Fabric platform](service-fabric-reliable-actors-platform.md).</span></span>

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
