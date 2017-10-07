---
title: "aaaCreate din första aktören-baserad Azure mikrotjänster i Java | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom hello stegen för att skapa, felsöka och distribuera en enkel aktören-baserad tjänst med hjälp av Service Fabric Reliable Actors."
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
ms.openlocfilehash: 24718a8d7034360c53597f139169580f1a6ce732
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-reliable-actors"></a><span data-ttu-id="92105-103">Komma igång med Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="92105-103">Getting started with Reliable Actors</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="92105-104">C# i Windows</span><span class="sxs-lookup"><span data-stu-id="92105-104">C# on Windows</span></span>](service-fabric-reliable-actors-get-started.md)
> * [<span data-ttu-id="92105-105">Java i Linux</span><span class="sxs-lookup"><span data-stu-id="92105-105">Java on Linux</span></span>](service-fabric-reliable-actors-get-started-java.md)
> 
> 

<span data-ttu-id="92105-106">Den här artikeln beskriver hello grunderna i Azure Service Fabric Reliable Actors och vägleder dig genom att skapa och distribuera ett enkelt program tillförlitliga aktören i Java.</span><span class="sxs-lookup"><span data-stu-id="92105-106">This article explains hello basics of Azure Service Fabric Reliable Actors and walks you through creating and deploying a simple Reliable Actor application in Java.</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="92105-107">Installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="92105-107">Installation and setup</span></span>
<span data-ttu-id="92105-108">Innan du börjar kontrollera att du har hello Service Fabric-utvecklingsmiljö ställa in på din dator.</span><span class="sxs-lookup"><span data-stu-id="92105-108">Before you start, make sure you have hello Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="92105-109">Om du behöver tooset den, gå för[komma igång på Mac](service-fabric-get-started-mac.md) eller [komma igång med Linux](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="92105-109">If you need tooset it up, go too[getting started on Mac](service-fabric-get-started-mac.md) or [getting started on Linux](service-fabric-get-started-linux.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="92105-110">Grundläggande begrepp</span><span class="sxs-lookup"><span data-stu-id="92105-110">Basic concepts</span></span>
<span data-ttu-id="92105-111">tooget igång med Reliable Actors du bara behöver toounderstand några grundläggande begrepp:</span><span class="sxs-lookup"><span data-stu-id="92105-111">tooget started with Reliable Actors, you only need toounderstand a few basic concepts:</span></span>

* <span data-ttu-id="92105-112">**Aktören tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="92105-112">**Actor service**.</span></span> <span data-ttu-id="92105-113">Reliable Actors paketeras i Reliable Services som kan distribueras i hello Service Fabric-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="92105-113">Reliable Actors are packaged in Reliable Services that can be deployed in hello Service Fabric infrastructure.</span></span> <span data-ttu-id="92105-114">Aktören instanser aktiveras på en namngiven tjänstinstans.</span><span class="sxs-lookup"><span data-stu-id="92105-114">Actor instances are activated in a named service instance.</span></span>
* <span data-ttu-id="92105-115">**Aktören registrering**.</span><span class="sxs-lookup"><span data-stu-id="92105-115">**Actor registration**.</span></span> <span data-ttu-id="92105-116">Som med Reliable Services en tillförlitlig aktören tjänst behöver toobe som registrerats med hello Service Fabric runtime.</span><span class="sxs-lookup"><span data-stu-id="92105-116">As with Reliable Services, a Reliable Actor service needs toobe registered with hello Service Fabric runtime.</span></span> <span data-ttu-id="92105-117">Hello aktörstyp måste dessutom toobe som registrerats med hello aktören körning.</span><span class="sxs-lookup"><span data-stu-id="92105-117">In addition, hello actor type needs toobe registered with hello Actor runtime.</span></span>
* <span data-ttu-id="92105-118">**Aktören gränssnittet**.</span><span class="sxs-lookup"><span data-stu-id="92105-118">**Actor interface**.</span></span> <span data-ttu-id="92105-119">hello aktören gränssnittet är används toodefine ett starkt typifierad offentliga gränssnitt för en aktör.</span><span class="sxs-lookup"><span data-stu-id="92105-119">hello actor interface is used toodefine a strongly typed public interface of an actor.</span></span> <span data-ttu-id="92105-120">I hello tillförlitliga aktören modellen terminologi definierar hello aktören-gränssnittet hello typer av meddelanden som hello aktören kan förstå och bearbeta.</span><span class="sxs-lookup"><span data-stu-id="92105-120">In hello Reliable Actor model terminology, hello actor interface defines hello types of messages that hello actor can understand and process.</span></span> <span data-ttu-id="92105-121">hello aktören gränssnitt som används av andra aktörer och klientprogram för ”skicka” (asynkront) meddelanden toohello aktören.</span><span class="sxs-lookup"><span data-stu-id="92105-121">hello actor interface is used by other actors and client applications too"send" (asynchronously) messages toohello actor.</span></span> <span data-ttu-id="92105-122">Reliable Actors kan implementera flera gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="92105-122">Reliable Actors can implement multiple interfaces.</span></span>
* <span data-ttu-id="92105-123">**ActorProxy klassen**.</span><span class="sxs-lookup"><span data-stu-id="92105-123">**ActorProxy class**.</span></span> <span data-ttu-id="92105-124">Hej ActorProxy klass som används av klienten program tooinvoke hello metoder som exponeras via hello aktören gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="92105-124">hello ActorProxy class is used by client applications tooinvoke hello methods exposed through hello actor interface.</span></span> <span data-ttu-id="92105-125">Hej ActorProxy klassen innehåller två viktiga funktioner:</span><span class="sxs-lookup"><span data-stu-id="92105-125">hello ActorProxy class provides two important functionalities:</span></span>
  
  * <span data-ttu-id="92105-126">Namnmatchning: det är kan toolocate hello aktören i hello kluster (Sök hello-nod i hello kluster där den finns).</span><span class="sxs-lookup"><span data-stu-id="92105-126">Name resolution: It is able toolocate hello actor in hello cluster (find hello node of hello cluster where it is hosted).</span></span>
  * <span data-ttu-id="92105-127">Hantering av fel: den gör anrop av metoden och nytt lösa hello aktören platsen efter, t.ex, ett fel som kräver hello aktören toobe flyttas tooanother nod i klustret för hello.</span><span class="sxs-lookup"><span data-stu-id="92105-127">Failure handling: It can retry method invocations and re-resolve hello actor location after, for example, a failure that requires hello actor toobe relocated tooanother node in hello cluster.</span></span>

<span data-ttu-id="92105-128">hello enligt reglerna som gäller tooactor gränssnitt är värt att nämna:</span><span class="sxs-lookup"><span data-stu-id="92105-128">hello following rules that pertain tooactor interfaces are worth mentioning:</span></span>

* <span data-ttu-id="92105-129">Aktören gränssnittsmetoder kan vara överbelastad.</span><span class="sxs-lookup"><span data-stu-id="92105-129">Actor interface methods cannot be overloaded.</span></span>
* <span data-ttu-id="92105-130">Aktören gränssnittet metoder inte får ha ut, ref eller valfria parametrar.</span><span class="sxs-lookup"><span data-stu-id="92105-130">Actor interface methods must not have out, ref, or optional parameters.</span></span>
* <span data-ttu-id="92105-131">Allmänt gränssnitt stöds inte.</span><span class="sxs-lookup"><span data-stu-id="92105-131">Generic interfaces are not supported.</span></span>

## <a name="create-an-actor-service"></a><span data-ttu-id="92105-132">Skapa en aktören service</span><span class="sxs-lookup"><span data-stu-id="92105-132">Create an actor service</span></span>
<span data-ttu-id="92105-133">Börja med att skapa ett nytt Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="92105-133">Start by creating a new Service Fabric application.</span></span> <span data-ttu-id="92105-134">hello Service Fabric-SDK för Linux innehåller en Yeoman generator tooprovide hello scaffold-teknik för ett Service Fabric-program med en tillståndslös tjänst.</span><span class="sxs-lookup"><span data-stu-id="92105-134">hello Service Fabric SDK for Linux includes a Yeoman generator tooprovide hello scaffolding for a Service Fabric application with a stateless service.</span></span> <span data-ttu-id="92105-135">Starta genom att köra hello följande Yeoman kommando:</span><span class="sxs-lookup"><span data-stu-id="92105-135">Start by running hello following Yeoman command:</span></span>

```bash
$ yo azuresfjava
```

<span data-ttu-id="92105-136">Följ hello instruktioner toocreate en **tillförlitliga aktören tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="92105-136">Follow hello instructions toocreate a **Reliable Actor Service**.</span></span> <span data-ttu-id="92105-137">Namn för den här självstudiekursen hello programmet ”HelloWorldActorApplication” och hello aktör ”HelloWorldActor”.</span><span class="sxs-lookup"><span data-stu-id="92105-137">For this tutorial, name hello application "HelloWorldActorApplication" and hello actor "HelloWorldActor."</span></span> <span data-ttu-id="92105-138">hello följande scaffold-teknik kommer att skapas:</span><span class="sxs-lookup"><span data-stu-id="92105-138">hello following scaffolding will be created:</span></span>

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

## <a name="reliable-actors-basic-building-blocks"></a><span data-ttu-id="92105-139">Tillförlitliga aktörer grundläggande byggstenarna</span><span class="sxs-lookup"><span data-stu-id="92105-139">Reliable Actors basic building blocks</span></span>
<span data-ttu-id="92105-140">hello grundläggande koncept beskrivs tidigare översätta till hello grundläggande byggstenarna för en tillförlitlig aktören-tjänst.</span><span class="sxs-lookup"><span data-stu-id="92105-140">hello basic concepts described earlier translate into hello basic building blocks of a Reliable Actor service.</span></span>

### <a name="actor-interface"></a><span data-ttu-id="92105-141">Aktören gränssnitt</span><span class="sxs-lookup"><span data-stu-id="92105-141">Actor interface</span></span>
<span data-ttu-id="92105-142">Hello gränssnittsdefinition för hello aktören innehåller.</span><span class="sxs-lookup"><span data-stu-id="92105-142">This contains hello interface definition for hello actor.</span></span> <span data-ttu-id="92105-143">Det här gränssnittet definierar hello aktören kontrakt som delas av hello aktören implementering och hello klienter anropar hello aktören, så gör det vanligtvis meningsfullt toodefine den på en plats som är separata från hello aktören implementering och kan delas av flera andra tjänster eller program.</span><span class="sxs-lookup"><span data-stu-id="92105-143">This interface defines hello actor contract that is shared by hello actor implementation and hello clients calling hello actor, so it typically makes sense toodefine it in a place that is separate from hello actor implementation and can be shared by multiple other services or client applications.</span></span>

<span data-ttu-id="92105-144">`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:</span><span class="sxs-lookup"><span data-stu-id="92105-144">`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:</span></span>

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a><span data-ttu-id="92105-145">Aktören service</span><span class="sxs-lookup"><span data-stu-id="92105-145">Actor service</span></span>
<span data-ttu-id="92105-146">Innehåller den aktören implementeringen och aktören Registreringskod.</span><span class="sxs-lookup"><span data-stu-id="92105-146">This contains your actor implementation and actor registration code.</span></span> <span data-ttu-id="92105-147">hello aktören klassen implementerar hello aktören gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="92105-147">hello actor class implements hello actor interface.</span></span> <span data-ttu-id="92105-148">Det är där din aktören sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="92105-148">This is where your actor does its work.</span></span>

<span data-ttu-id="92105-149">`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:</span><span class="sxs-lookup"><span data-stu-id="92105-149">`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:</span></span>

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

### <a name="actor-registration"></a><span data-ttu-id="92105-150">Aktören registrering</span><span class="sxs-lookup"><span data-stu-id="92105-150">Actor registration</span></span>
<span data-ttu-id="92105-151">hello aktören service måste ha registrerats med en typ i hello Service Fabric-körningsmiljön.</span><span class="sxs-lookup"><span data-stu-id="92105-151">hello actor service must be registered with a service type in hello Service Fabric runtime.</span></span> <span data-ttu-id="92105-152">I ordning för hello aktören Service toorun aktören-instanser, aktören-typen måste också vara registrerad med hello aktören Service.</span><span class="sxs-lookup"><span data-stu-id="92105-152">In order for hello Actor Service toorun your actor instances, your actor type must also be registered with hello Actor Service.</span></span> <span data-ttu-id="92105-153">Hej `ActorRuntime` registreringsmetod utför detta arbete för aktörer.</span><span class="sxs-lookup"><span data-stu-id="92105-153">hello `ActorRuntime` registration method performs this work for actors.</span></span>

<span data-ttu-id="92105-154">`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:</span><span class="sxs-lookup"><span data-stu-id="92105-154">`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:</span></span>

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

### <a name="test-client"></a><span data-ttu-id="92105-155">Testklienten</span><span class="sxs-lookup"><span data-stu-id="92105-155">Test client</span></span>
<span data-ttu-id="92105-156">Detta är ett enkelt test-klientprogram kan du köra separat från hello Service Fabric application tootest aktören-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="92105-156">This is a simple test client application you can run separately from hello Service Fabric application tootest your actor service.</span></span> <span data-ttu-id="92105-157">Detta är ett exempel på där hello ActorProxy kan använda tooactivate och kommunicera med aktören instanser.</span><span class="sxs-lookup"><span data-stu-id="92105-157">This is an example of where hello ActorProxy can be used tooactivate and communicate with actor instances.</span></span> <span data-ttu-id="92105-158">Det inte distribueras med din tjänst.</span><span class="sxs-lookup"><span data-stu-id="92105-158">It does not get deployed with your service.</span></span>

### <a name="hello-application"></a><span data-ttu-id="92105-159">hello-program</span><span class="sxs-lookup"><span data-stu-id="92105-159">hello application</span></span>
<span data-ttu-id="92105-160">Slutligen hello hello programpaket aktören tjänsten och andra tjänster som du kan lägga till i hello framtida tillsammans för distribution.</span><span class="sxs-lookup"><span data-stu-id="92105-160">Finally, hello application packages hello actor service and any other services you might add in hello future together for deployment.</span></span> <span data-ttu-id="92105-161">Den innehåller hello *ApplicationManifest.xml* och platshållare för hello aktören service-paketet.</span><span class="sxs-lookup"><span data-stu-id="92105-161">It contains hello *ApplicationManifest.xml* and place holders for hello actor service package.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="92105-162">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="92105-162">Run hello application</span></span>

<span data-ttu-id="92105-163">Hej Yeoman scaffold-teknik som omfattar en gradle skriptet toobuild hello program och bash-skript toodeploy och ta bort programmet.</span><span class="sxs-lookup"><span data-stu-id="92105-163">hello Yeoman scaffolding includes a gradle script toobuild hello application and bash scripts toodeploy and remove the application.</span></span> <span data-ttu-id="92105-164">toodeploy hello program, första build hello med gradle:</span><span class="sxs-lookup"><span data-stu-id="92105-164">toodeploy hello application, first build hello application with gradle:</span></span>

```bash
$ gradle
```

<span data-ttu-id="92105-165">Detta genererar ett Service Fabric-programpaket som kan distribueras med hjälp av Service Fabric CLI-verktygen.</span><span class="sxs-lookup"><span data-stu-id="92105-165">This will produce a Service Fabric application package that can be deployed using Service Fabric CLI tools.</span></span>

### <a name="deploy-service-fabric-cli"></a><span data-ttu-id="92105-166">Distribuera Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="92105-166">Deploy Service Fabric CLI</span></span>

<span data-ttu-id="92105-167">Hej install.sh skriptet innehåller hello nödvändiga Service Fabric CLI (sfctl) kommandon toodeploy hello programpaket.</span><span class="sxs-lookup"><span data-stu-id="92105-167">hello install.sh script contains hello necessary Service Fabric CLI (sfctl) commands toodeploy hello application package.</span></span>
<span data-ttu-id="92105-168">Kör hello install.sh skriptet toodeploy hello program.</span><span class="sxs-lookup"><span data-stu-id="92105-168">Run hello install.sh script toodeploy hello application.</span></span>

```bash
$ ./install.sh
```

## <a name="next-steps"></a><span data-ttu-id="92105-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="92105-169">Next steps</span></span>

* [<span data-ttu-id="92105-170">Kom igång med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="92105-170">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
