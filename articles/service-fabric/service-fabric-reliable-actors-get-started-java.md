---
title: "Skapa din första Azure aktören-baserade-mikrotjänster i Java | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom stegen för att skapa, felsöka och distribuera en enkel aktören-baserad tjänst med hjälp av Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d31dc8ab-9760-4619-a641-facb8324c759
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2017
ms.author: vturecek
ms.openlocfilehash: 288f1ed1016f50031065e66444d2562427194dc7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-reliable-actors"></a><span data-ttu-id="37ce3-103">Komma igång med Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="37ce3-103">Getting started with Reliable Actors</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="37ce3-104">C# i Windows</span><span class="sxs-lookup"><span data-stu-id="37ce3-104">C# on Windows</span></span>](service-fabric-reliable-actors-get-started.md)
> * [<span data-ttu-id="37ce3-105">Java i Linux</span><span class="sxs-lookup"><span data-stu-id="37ce3-105">Java on Linux</span></span>](service-fabric-reliable-actors-get-started-java.md)
> 
> 

<span data-ttu-id="37ce3-106">Den här artikeln beskriver grunderna i Azure Service Fabric Reliable Actors och vägleder dig genom att skapa och distribuera ett enkelt program tillförlitliga aktören i Java.</span><span class="sxs-lookup"><span data-stu-id="37ce3-106">This article explains the basics of Azure Service Fabric Reliable Actors and walks you through creating and deploying a simple Reliable Actor application in Java.</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="37ce3-107">Installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="37ce3-107">Installation and setup</span></span>
<span data-ttu-id="37ce3-108">Innan du börjar bör du kontrollera du har Service Fabric-utvecklingsmiljö ställa in på din dator.</span><span class="sxs-lookup"><span data-stu-id="37ce3-108">Before you start, make sure you have the Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="37ce3-109">Om du behöver konfigurera den går du till [komma igång på Mac](service-fabric-get-started-mac.md) eller [komma igång med Linux](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="37ce3-109">If you need to set it up, go to [getting started on Mac](service-fabric-get-started-mac.md) or [getting started on Linux](service-fabric-get-started-linux.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="37ce3-110">Grundläggande begrepp</span><span class="sxs-lookup"><span data-stu-id="37ce3-110">Basic concepts</span></span>
<span data-ttu-id="37ce3-111">Om du vill komma igång med Reliable Actors, behöver du bara förstå några grundläggande begrepp:</span><span class="sxs-lookup"><span data-stu-id="37ce3-111">To get started with Reliable Actors, you only need to understand a few basic concepts:</span></span>

* <span data-ttu-id="37ce3-112">**Aktören tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="37ce3-112">**Actor service**.</span></span> <span data-ttu-id="37ce3-113">Reliable Actors paketeras i Reliable Services som kan distribueras i Service Fabric-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="37ce3-113">Reliable Actors are packaged in Reliable Services that can be deployed in the Service Fabric infrastructure.</span></span> <span data-ttu-id="37ce3-114">Aktören instanser aktiveras på en namngiven tjänstinstans.</span><span class="sxs-lookup"><span data-stu-id="37ce3-114">Actor instances are activated in a named service instance.</span></span>
* <span data-ttu-id="37ce3-115">**Aktören registrering**.</span><span class="sxs-lookup"><span data-stu-id="37ce3-115">**Actor registration**.</span></span> <span data-ttu-id="37ce3-116">Som med Reliable Services måste en tillförlitlig aktören tjänst vara registrerad med Service Fabric-körning.</span><span class="sxs-lookup"><span data-stu-id="37ce3-116">As with Reliable Services, a Reliable Actor service needs to be registered with the Service Fabric runtime.</span></span> <span data-ttu-id="37ce3-117">Dessutom måste aktören-typen vara registrerad med aktören körningsmiljön.</span><span class="sxs-lookup"><span data-stu-id="37ce3-117">In addition, the actor type needs to be registered with the Actor runtime.</span></span>
* <span data-ttu-id="37ce3-118">**Aktören gränssnittet**.</span><span class="sxs-lookup"><span data-stu-id="37ce3-118">**Actor interface**.</span></span> <span data-ttu-id="37ce3-119">Gränssnittet aktören används för att definiera en strikt typkontroll offentliga gränssnittet för en aktör.</span><span class="sxs-lookup"><span data-stu-id="37ce3-119">The actor interface is used to define a strongly typed public interface of an actor.</span></span> <span data-ttu-id="37ce3-120">I den tillförlitliga aktören modellen terminologin definierar aktören-gränssnittet vilka typer av meddelanden som aktören kan förstå och processen.</span><span class="sxs-lookup"><span data-stu-id="37ce3-120">In the Reliable Actor model terminology, the actor interface defines the types of messages that the actor can understand and process.</span></span> <span data-ttu-id="37ce3-121">Gränssnittet aktören används av andra aktörer och klientprogram för att ”skicka” (asynkront) meddelanden för aktören.</span><span class="sxs-lookup"><span data-stu-id="37ce3-121">The actor interface is used by other actors and client applications to "send" (asynchronously) messages to the actor.</span></span> <span data-ttu-id="37ce3-122">Reliable Actors kan implementera flera gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="37ce3-122">Reliable Actors can implement multiple interfaces.</span></span>
* <span data-ttu-id="37ce3-123">**ActorProxy klassen**.</span><span class="sxs-lookup"><span data-stu-id="37ce3-123">**ActorProxy class**.</span></span> <span data-ttu-id="37ce3-124">Klassen ActorProxy används av klientprogram för att anropa metoder som exponeras via gränssnittet aktören.</span><span class="sxs-lookup"><span data-stu-id="37ce3-124">The ActorProxy class is used by client applications to invoke the methods exposed through the actor interface.</span></span> <span data-ttu-id="37ce3-125">Klassen ActorProxy innehåller två viktiga funktioner:</span><span class="sxs-lookup"><span data-stu-id="37ce3-125">The ActorProxy class provides two important functionalities:</span></span>
  
  * <span data-ttu-id="37ce3-126">Namnmatchning: det är att hitta aktören i klustret (hitta nod i klustret där den finns).</span><span class="sxs-lookup"><span data-stu-id="37ce3-126">Name resolution: It is able to locate the actor in the cluster (find the node of the cluster where it is hosted).</span></span>
  * <span data-ttu-id="37ce3-127">Hantering av fel: den kan gör anrop av metoden och lösa aktören platsen igen efter, till exempel ett fel som kräver aktören att flyttas till en annan nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="37ce3-127">Failure handling: It can retry method invocations and re-resolve the actor location after, for example, a failure that requires the actor to be relocated to another node in the cluster.</span></span>

<span data-ttu-id="37ce3-128">Följande regler som hör till aktören gränssnitt är värt att nämna:</span><span class="sxs-lookup"><span data-stu-id="37ce3-128">The following rules that pertain to actor interfaces are worth mentioning:</span></span>

* <span data-ttu-id="37ce3-129">Aktören gränssnittsmetoder kan vara överbelastad.</span><span class="sxs-lookup"><span data-stu-id="37ce3-129">Actor interface methods cannot be overloaded.</span></span>
* <span data-ttu-id="37ce3-130">Aktören gränssnittet metoder inte får ha ut, ref eller valfria parametrar.</span><span class="sxs-lookup"><span data-stu-id="37ce3-130">Actor interface methods must not have out, ref, or optional parameters.</span></span>
* <span data-ttu-id="37ce3-131">Allmänt gränssnitt stöds inte.</span><span class="sxs-lookup"><span data-stu-id="37ce3-131">Generic interfaces are not supported.</span></span>

## <a name="create-an-actor-service"></a><span data-ttu-id="37ce3-132">Skapa en aktören service</span><span class="sxs-lookup"><span data-stu-id="37ce3-132">Create an actor service</span></span>
<span data-ttu-id="37ce3-133">Börja med att skapa ett nytt Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="37ce3-133">Start by creating a new Service Fabric application.</span></span> <span data-ttu-id="37ce3-134">Service Fabric-SDK för Linux innehåller en Yeoman generator att tillhandahålla scaffold-teknik för ett Service Fabric-program med en tillståndslös tjänst.</span><span class="sxs-lookup"><span data-stu-id="37ce3-134">The Service Fabric SDK for Linux includes a Yeoman generator to provide the scaffolding for a Service Fabric application with a stateless service.</span></span> <span data-ttu-id="37ce3-135">Starta genom att köra följande Yeoman kommando:</span><span class="sxs-lookup"><span data-stu-id="37ce3-135">Start by running the following Yeoman command:</span></span>

```bash
$ yo azuresfjava
```

<span data-ttu-id="37ce3-136">Följ instruktionerna för att skapa en **tillförlitliga aktören tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="37ce3-136">Follow the instructions to create a **Reliable Actor Service**.</span></span> <span data-ttu-id="37ce3-137">Namn för programmet ”HelloWorldActorApplication” för den här självstudiekursen och aktör ”HelloWorldActor”.</span><span class="sxs-lookup"><span data-stu-id="37ce3-137">For this tutorial, name the application "HelloWorldActorApplication" and the actor "HelloWorldActor."</span></span> <span data-ttu-id="37ce3-138">Scaffold-teknik för följande skapas:</span><span class="sxs-lookup"><span data-stu-id="37ce3-138">The following scaffolding will be created:</span></span>

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a><span data-ttu-id="37ce3-139">Tillförlitliga aktörer grundläggande byggstenarna</span><span class="sxs-lookup"><span data-stu-id="37ce3-139">Reliable Actors basic building blocks</span></span>
<span data-ttu-id="37ce3-140">Grundläggande begrepp som beskrivs ovan översätta till de grundläggande byggstenarna för en tillförlitlig aktören-tjänst.</span><span class="sxs-lookup"><span data-stu-id="37ce3-140">The basic concepts described earlier translate into the basic building blocks of a Reliable Actor service.</span></span>

### <a name="actor-interface"></a><span data-ttu-id="37ce3-141">Aktören gränssnitt</span><span class="sxs-lookup"><span data-stu-id="37ce3-141">Actor interface</span></span>
<span data-ttu-id="37ce3-142">Innehåller gränssnittsdefinition för aktören.</span><span class="sxs-lookup"><span data-stu-id="37ce3-142">This contains the interface definition for the actor.</span></span> <span data-ttu-id="37ce3-143">Det här gränssnittet definierar aktören kontraktet som delas av aktören implementering och klienterna som anropar aktören så vanligtvis är det praktiskt att definiera den på en plats som är separat från aktören implementering och kan delas av flera tjänster eller klientprogram.</span><span class="sxs-lookup"><span data-stu-id="37ce3-143">This interface defines the actor contract that is shared by the actor implementation and the clients calling the actor, so it typically makes sense to define it in a place that is separate from the actor implementation and can be shared by multiple other services or client applications.</span></span>

<span data-ttu-id="37ce3-144">`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:</span><span class="sxs-lookup"><span data-stu-id="37ce3-144">`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:</span></span>

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a><span data-ttu-id="37ce3-145">Aktören service</span><span class="sxs-lookup"><span data-stu-id="37ce3-145">Actor service</span></span>
<span data-ttu-id="37ce3-146">Innehåller den aktören implementeringen och aktören Registreringskod.</span><span class="sxs-lookup"><span data-stu-id="37ce3-146">This contains your actor implementation and actor registration code.</span></span> <span data-ttu-id="37ce3-147">Aktören-klassen implementerar gränssnittet aktören.</span><span class="sxs-lookup"><span data-stu-id="37ce3-147">The actor class implements the actor interface.</span></span> <span data-ttu-id="37ce3-148">Det är där din aktören sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="37ce3-148">This is where your actor does its work.</span></span>

<span data-ttu-id="37ce3-149">`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:</span><span class="sxs-lookup"><span data-stu-id="37ce3-149">`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:</span></span>

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a><span data-ttu-id="37ce3-150">Aktören registrering</span><span class="sxs-lookup"><span data-stu-id="37ce3-150">Actor registration</span></span>
<span data-ttu-id="37ce3-151">Tjänsten aktören måste ha registrerats med en typ i Service Fabric-körningsmiljön.</span><span class="sxs-lookup"><span data-stu-id="37ce3-151">The actor service must be registered with a service type in the Service Fabric runtime.</span></span> <span data-ttu-id="37ce3-152">För att tjänsten aktören körs aktören-instanser måste aktörstyp registreras med tjänsten aktören.</span><span class="sxs-lookup"><span data-stu-id="37ce3-152">In order for the Actor Service to run your actor instances, your actor type must also be registered with the Actor Service.</span></span> <span data-ttu-id="37ce3-153">Den `ActorRuntime` registreringsmetod utför detta arbete för aktörer.</span><span class="sxs-lookup"><span data-stu-id="37ce3-153">The `ActorRuntime` registration method performs this work for actors.</span></span>

<span data-ttu-id="37ce3-154">`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:</span><span class="sxs-lookup"><span data-stu-id="37ce3-154">`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:</span></span>

```java
public class HelloWorldActorHost {

    public static void main(String[] args) throws Exception {

        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);

        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a><span data-ttu-id="37ce3-155">Testklienten</span><span class="sxs-lookup"><span data-stu-id="37ce3-155">Test client</span></span>
<span data-ttu-id="37ce3-156">Detta är ett enkelt test-klientprogram kan du köra separat från Service Fabric-programmet för att testa aktören tjänsten.</span><span class="sxs-lookup"><span data-stu-id="37ce3-156">This is a simple test client application you can run separately from the Service Fabric application to test your actor service.</span></span> <span data-ttu-id="37ce3-157">Detta är ett exempel där ActorProxy kan användas för att aktivera och kommunicera med aktören instanser.</span><span class="sxs-lookup"><span data-stu-id="37ce3-157">This is an example of where the ActorProxy can be used to activate and communicate with actor instances.</span></span> <span data-ttu-id="37ce3-158">Det inte distribueras med din tjänst.</span><span class="sxs-lookup"><span data-stu-id="37ce3-158">It does not get deployed with your service.</span></span>

### <a name="the-application"></a><span data-ttu-id="37ce3-159">Programmet</span><span class="sxs-lookup"><span data-stu-id="37ce3-159">The application</span></span>
<span data-ttu-id="37ce3-160">Slutligen paket programmet för tjänsten aktören och andra tjänster som du kan lägga till i framtiden tillsammans för distribution.</span><span class="sxs-lookup"><span data-stu-id="37ce3-160">Finally, the application packages the actor service and any other services you might add in the future together for deployment.</span></span> <span data-ttu-id="37ce3-161">Den innehåller den *ApplicationManifest.xml* och platshållare för servicepaket aktören.</span><span class="sxs-lookup"><span data-stu-id="37ce3-161">It contains the *ApplicationManifest.xml* and place holders for the actor service package.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="37ce3-162">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="37ce3-162">Run the application</span></span>

<span data-ttu-id="37ce3-163">Yeoman scaffold-teknik innehåller ett gradle-skript för att skapa programmet och bash-skript för att distribuera och ta bort programmet.</span><span class="sxs-lookup"><span data-stu-id="37ce3-163">The Yeoman scaffolding includes a gradle script to build the application and bash scripts to deploy and remove the application.</span></span> <span data-ttu-id="37ce3-164">Om du vill distribuera programmet först skapa programmet med gradle:</span><span class="sxs-lookup"><span data-stu-id="37ce3-164">To deploy the application, first build the application with gradle:</span></span>

```bash
$ gradle
```

<span data-ttu-id="37ce3-165">Detta genererar ett Service Fabric-programpaket som kan distribueras med hjälp av Service Fabric CLI-verktygen.</span><span class="sxs-lookup"><span data-stu-id="37ce3-165">This will produce a Service Fabric application package that can be deployed using Service Fabric CLI tools.</span></span>

### <a name="deploy-service-fabric-cli"></a><span data-ttu-id="37ce3-166">Distribuera Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="37ce3-166">Deploy Service Fabric CLI</span></span>

<span data-ttu-id="37ce3-167">Skriptet install.sh innehåller de nödvändiga Service Fabric CLI (sfctl)-kommandona för att distribuera programpaketet.</span><span class="sxs-lookup"><span data-stu-id="37ce3-167">The install.sh script contains the necessary Service Fabric CLI (sfctl) commands to deploy the application package.</span></span>
<span data-ttu-id="37ce3-168">Kör skriptet install.sh om du vill distribuera programmet.</span><span class="sxs-lookup"><span data-stu-id="37ce3-168">Run the install.sh script to deploy the application.</span></span>

```bash
$ ./install.sh
```

## <a name="next-steps"></a><span data-ttu-id="37ce3-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37ce3-169">Next steps</span></span>

* [<span data-ttu-id="37ce3-170">Kom igång med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="37ce3-170">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
