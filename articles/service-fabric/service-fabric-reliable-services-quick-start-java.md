---
title: "aaaCreate din första tillförlitliga mikrotjänster i Java Azure | Microsoft Docs"
description: "Introduktion toocreating ett Microsoft Azure Service Fabric-program med tillståndslösa och tillståndskänsliga tjänster."
services: service-fabric
documentationcenter: java
author: vturecek
manager: timlt
editor: 
ms.assetid: 7831886f-7ec4-4aef-95c5-b2469a5b7b5d
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 577d96591797bbfe6be5c1094426b5f1435cca0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-reliable-services"></a><span data-ttu-id="1e1c6-103">Kom igång med Reliable Services</span><span class="sxs-lookup"><span data-stu-id="1e1c6-103">Get started with Reliable Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1e1c6-104">C# i Windows</span><span class="sxs-lookup"><span data-stu-id="1e1c6-104">C# on Windows</span></span>](service-fabric-reliable-services-quick-start.md)
> * [<span data-ttu-id="1e1c6-105">Java i Linux</span><span class="sxs-lookup"><span data-stu-id="1e1c6-105">Java on Linux</span></span>](service-fabric-reliable-services-quick-start-java.md)
>
>

<span data-ttu-id="1e1c6-106">Den här artikeln beskriver hello grunderna i Azure Service Fabric Reliable Services och vägleder dig genom att skapa och distribuera en enkel tillförlitliga tjänstprogrammet skriven i Java.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-106">This article explains hello basics of Azure Service Fabric Reliable Services and walks you through creating and deploying a simple Reliable Service application written in Java.</span></span> <span data-ttu-id="1e1c6-107">Den här videon Microsoft Virtual Academy visar också hur toocreate en tillståndslös tillförlitlig tjänst:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965"></span><span class="sxs-lookup"><span data-stu-id="1e1c6-107">This Microsoft Virtual Academy video also shows you how toocreate a stateless Reliable service: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965"></span></span>  
<img src="./media/service-fabric-reliable-services-quick-start-java/ReliableServicesJavaVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="installation-and-setup"></a><span data-ttu-id="1e1c6-108">Installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="1e1c6-108">Installation and setup</span></span>
<span data-ttu-id="1e1c6-109">Innan du börjar kontrollera att du har hello Service Fabric-utvecklingsmiljö ställa in på din dator.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-109">Before you start, make sure you have hello Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="1e1c6-110">Om du behöver tooset den, gå för[komma igång på Mac](service-fabric-get-started-mac.md) eller [komma igång med Linux](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1e1c6-110">If you need tooset it up, go too[getting started on Mac](service-fabric-get-started-mac.md) or [getting started on Linux](service-fabric-get-started-linux.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="1e1c6-111">Grundläggande begrepp</span><span class="sxs-lookup"><span data-stu-id="1e1c6-111">Basic concepts</span></span>
<span data-ttu-id="1e1c6-112">tooget igång med tillförlitlig tjänster kan du bara behöver toounderstand några grundläggande begrepp:</span><span class="sxs-lookup"><span data-stu-id="1e1c6-112">tooget started with Reliable Services, you only need toounderstand a few basic concepts:</span></span>

* <span data-ttu-id="1e1c6-113">**Typen tjänst**: det här är din implementering.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-113">**Service type**: This is your service implementation.</span></span> <span data-ttu-id="1e1c6-114">Den definieras av hello-klass som du skriver och som utökar `StatelessService` och annan kod eller beroenden, används tillsammans med ett namn och ett versionsnummer.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-114">It is defined by hello class you write that extends `StatelessService` and any other code or dependencies used therein, along with a name and a version number.</span></span>
* <span data-ttu-id="1e1c6-115">**Med namnet tjänstinstans**: toorun din tjänst du skapar namngivna instanser av service-typen mycket som du skapar objektinstanser av en klasstyp.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-115">**Named service instance**: toorun your service, you create named instances of your service type, much like you create object instances of a class type.</span></span> <span data-ttu-id="1e1c6-116">Instanser av tjänsten är i själva verket objektinstansieringar av din tjänstklass som du skriver.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-116">Service instances are in fact object instantiations of your service class that you write.</span></span>
* <span data-ttu-id="1e1c6-117">**Tjänstvärden**: hello med namnet tjänstinstanser som du skapar måste toorun inuti en värd.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-117">**Service host**: hello named service instances you create need toorun inside a host.</span></span> <span data-ttu-id="1e1c6-118">hello tjänstvärden är bara en process där instanser av tjänsten kan köras.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-118">hello service host is just a process where instances of your service can run.</span></span>
* <span data-ttu-id="1e1c6-119">**Registrering av tjänst**: registrering sammanför allt.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-119">**Service registration**: Registration brings everything together.</span></span> <span data-ttu-id="1e1c6-120">hello service-typen måste vara registrerad med hello Service Fabric runtime i en tjänst värd tooallow Service Fabric toocreate instanser av den toorun.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-120">hello service type must be registered with hello Service Fabric runtime in a service host tooallow Service Fabric toocreate instances of it toorun.</span></span>  

## <a name="create-a-stateless-service"></a><span data-ttu-id="1e1c6-121">Skapa en tillståndslös tjänst</span><span class="sxs-lookup"><span data-stu-id="1e1c6-121">Create a stateless service</span></span>
<span data-ttu-id="1e1c6-122">Börja med att skapa ett Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-122">Start by creating a Service Fabric application.</span></span> <span data-ttu-id="1e1c6-123">hello Service Fabric-SDK för Linux innehåller en Yeoman generator tooprovide hello scaffold-teknik för ett Service Fabric-program med en tillståndslös tjänst.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-123">hello Service Fabric SDK for Linux includes a Yeoman generator tooprovide hello scaffolding for a Service Fabric application with a stateless service.</span></span> <span data-ttu-id="1e1c6-124">Starta genom att köra hello följande Yeoman kommando:</span><span class="sxs-lookup"><span data-stu-id="1e1c6-124">Start by running hello following Yeoman command:</span></span>

```bash
$ yo azuresfjava
```

<span data-ttu-id="1e1c6-125">Följ hello instruktioner toocreate en **tillförlitliga tillståndslösa tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-125">Follow hello instructions toocreate a **Reliable Stateless Service**.</span></span> <span data-ttu-id="1e1c6-126">Den här självstudien namn hello programmet ”HelloWorldApplication” och hello-tjänsten ”HelloWorld”.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-126">For this tutorial, name hello application "HelloWorldApplication" and hello service "HelloWorld".</span></span> <span data-ttu-id="1e1c6-127">omfattar hello resultatet kataloger för hello `HelloWorldApplication` och `HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-127">hello result includes directories for hello `HelloWorldApplication` and `HelloWorld`.</span></span>

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-hello-service"></a><span data-ttu-id="1e1c6-128">Implementera hello-tjänsten</span><span class="sxs-lookup"><span data-stu-id="1e1c6-128">Implement hello service</span></span>
<span data-ttu-id="1e1c6-129">Öppna **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-129">Open **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**.</span></span> <span data-ttu-id="1e1c6-130">Den här klassen definierar hello service-typen och köra all kod.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-130">This class defines hello service type, and can run any code.</span></span> <span data-ttu-id="1e1c6-131">hello service API innehåller två startpunkter för din kod:</span><span class="sxs-lookup"><span data-stu-id="1e1c6-131">hello service API provides two entry points for your code:</span></span>

* <span data-ttu-id="1e1c6-132">En öppen metoden, kallas `runAsync()`, där du kan börja köra alla arbetsbelastningar, inklusive tidskrävande beräkning av arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-132">An open-ended entry point method, called `runAsync()`, where you can begin executing any workloads, including long-running compute workloads.</span></span>

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* <span data-ttu-id="1e1c6-133">En kommunikation startpunkt där du kan ansluta i din kommunikation stack med val.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-133">A communication entry point where you can plug in your communication stack of choice.</span></span> <span data-ttu-id="1e1c6-134">Det är där du kan börja ta emot begäranden från användare och andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-134">This is where you can start receiving requests from users and other services.</span></span>

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

<span data-ttu-id="1e1c6-135">I den här självstudiekursen kommer vi fokusera på hello `runAsync()` metoden.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-135">In this tutorial, we focus on hello `runAsync()` entry point method.</span></span> <span data-ttu-id="1e1c6-136">Det är där du kan starta direkt köra din kod.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-136">This is where you can immediately start running your code.</span></span>

### <a name="runasync"></a><span data-ttu-id="1e1c6-137">RunAsync</span><span class="sxs-lookup"><span data-stu-id="1e1c6-137">RunAsync</span></span>
<span data-ttu-id="1e1c6-138">hello plattform anropar den här metoden när en instans av en tjänst är monterade och klar tooexecute.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-138">hello platform calls this method when an instance of a service is placed and ready tooexecute.</span></span> <span data-ttu-id="1e1c6-139">För en tillståndslös tjänst innebär som bara när hello tjänstinstans öppnas.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-139">For a stateless service, that simply means when hello service instance is opened.</span></span> <span data-ttu-id="1e1c6-140">En token för annullering tillhandahålls toocoordinate när service-instans måste toobe stängd.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-140">A cancellation token is provided toocoordinate when your service instance needs toobe closed.</span></span> <span data-ttu-id="1e1c6-141">I Service Fabric inträffa cykeln Öppna/stänga av en tjänstinstans många gånger under hello livslängd för hello-tjänsten som helhet.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-141">In Service Fabric, this open/close cycle of a service instance can occur many times over hello lifetime of hello service as a whole.</span></span> <span data-ttu-id="1e1c6-142">Detta kan inträffa av olika orsaker, bland annat:</span><span class="sxs-lookup"><span data-stu-id="1e1c6-142">This can happen for various reasons, including:</span></span>

* <span data-ttu-id="1e1c6-143">hello flyttas instanser av tjänsten för resurser.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-143">hello system moves your service instances for resource balancing.</span></span>
* <span data-ttu-id="1e1c6-144">Fel uppstår i koden.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-144">Faults occur in your code.</span></span>
* <span data-ttu-id="1e1c6-145">hello programmets eller systemets uppgraderas.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-145">hello application or system is upgraded.</span></span>
* <span data-ttu-id="1e1c6-146">hello underliggande maskinvara påträffar ett avbrott.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-146">hello underlying hardware experiences an outage.</span></span>

<span data-ttu-id="1e1c6-147">Den här orchestration hanteras av Service Fabric tookeep tjänsten hög tillgänglighet och korrekt Balanserat.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-147">This orchestration is managed by Service Fabric tookeep your service highly available and properly balanced.</span></span>

<span data-ttu-id="1e1c6-148">`runAsync()`bör inte blockera synkront.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-148">`runAsync()` should not block synchronously.</span></span> <span data-ttu-id="1e1c6-149">Implementeringen av runAsync ska returnera en CompletableFuture tooallow hello runtime toocontinue.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-149">Your implementation of runAsync should return a CompletableFuture tooallow hello runtime toocontinue.</span></span> <span data-ttu-id="1e1c6-150">Om din arbetsbelastning behöver tooimplement en tidskrävande uppgift som bör göras i hello CompletableFuture.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-150">If your workload needs tooimplement a long running task that should be done inside hello CompletableFuture.</span></span>

#### <a name="cancellation"></a><span data-ttu-id="1e1c6-151">Annullering</span><span class="sxs-lookup"><span data-stu-id="1e1c6-151">Cancellation</span></span>
<span data-ttu-id="1e1c6-152">Annullering av din arbetsbelastning är en samverkande ansträngning styrd av hello tillhandahålls annullering token.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-152">Cancellation of your workload is a cooperative effort orchestrated by hello provided cancellation token.</span></span> <span data-ttu-id="1e1c6-153">hello systemet väntar aktiviteten-tooend (av lyckades, avbokning eller fel) innan den flyttas på.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-153">hello system waits for your task tooend (by successful completion, cancellation, or fault) before it moves on.</span></span> <span data-ttu-id="1e1c6-154">Det är viktigt toohonor hello annullering token, Slutför någon fungerar och avsluta `runAsync()` så snabbt som möjligt när hello systemet begär annullering.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-154">It is important toohonor hello cancellation token, finish any work, and exit `runAsync()` as quickly as possible when hello system requests cancellation.</span></span> <span data-ttu-id="1e1c6-155">hello exemplet nedan visar hur toohandle en annullering händelse:</span><span class="sxs-lookup"><span data-stu-id="1e1c6-155">hello following example demonstrates how toohandle a cancellation event:</span></span>

```java
    @Override
    protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

        // TODO: Replace hello following sample code with your own logic
        // or remove this runAsync override if it's not needed in your service.

        CompletableFuture.runAsync(() -> {
          long iterations = 0;
          while(true)
          {
            cancellationToken.throwIfCancellationRequested();
            logger.log(Level.INFO, "Working-{0}", ++iterations);

            try
            {
              Thread.sleep(1000);
            }
            catch (IOException ex) {}
          }
        });
    }
```

### <a name="service-registration"></a><span data-ttu-id="1e1c6-156">Tjänstregistrering</span><span class="sxs-lookup"><span data-stu-id="1e1c6-156">Service registration</span></span>
<span data-ttu-id="1e1c6-157">Tjänsttyper måste registreras med hello Service Fabric runtime.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-157">Service types must be registered with hello Service Fabric runtime.</span></span> <span data-ttu-id="1e1c6-158">hello service-typen är definierad i hello `ServiceManifest.xml` och tjänstklassen som implementerar `StatelessService`.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-158">hello service type is defined in hello `ServiceManifest.xml` and your service class that implements `StatelessService`.</span></span> <span data-ttu-id="1e1c6-159">Registreringen för tjänsten utförs i hello processen startpunkten.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-159">Service registration is performed in hello process main entry point.</span></span> <span data-ttu-id="1e1c6-160">I det här exemplet hello processen Huvudstartadressen är `HelloWorldServiceHost.java`:</span><span class="sxs-lookup"><span data-stu-id="1e1c6-160">In this example, hello process main entry point is `HelloWorldServiceHost.java`:</span></span>

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    }
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration:", ex);
        throw ex;
    }
}
```

## <a name="run-hello-application"></a><span data-ttu-id="1e1c6-161">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="1e1c6-161">Run hello application</span></span>

<span data-ttu-id="1e1c6-162">Hej Yeoman scaffold-teknik som omfattar en gradle skriptet toobuild hello program och bash-skript toodeploy och ta bort programmet.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-162">hello Yeoman scaffolding includes a gradle script toobuild hello application and bash scripts toodeploy and remove the application.</span></span> <span data-ttu-id="1e1c6-163">toorun hello program, första build hello med gradle:</span><span class="sxs-lookup"><span data-stu-id="1e1c6-163">toorun hello application, first build hello application with gradle:</span></span>

```bash
$ gradle
```

<span data-ttu-id="1e1c6-164">Detta ger ett Service Fabric-programpaket som kan distribueras med hjälp av Service Fabric CLI.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-164">This produces a Service Fabric application package that can be deployed using Service Fabric CLI.</span></span>

### <a name="deploy-with-service-fabric-cli"></a><span data-ttu-id="1e1c6-165">Distribuera med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="1e1c6-165">Deploy with Service Fabric CLI</span></span>

<span data-ttu-id="1e1c6-166">Hej install.sh skriptet innehåller hello nödvändiga Service Fabric CLI-kommandon toodeploy hello programpaket.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-166">hello install.sh script contains hello necessary Service Fabric CLI commands toodeploy hello application package.</span></span> <span data-ttu-id="1e1c6-167">Kör install.sh skriptet toodeploy hello program.</span><span class="sxs-lookup"><span data-stu-id="1e1c6-167">Run the install.sh script toodeploy hello application.</span></span>

```bash
$ ./install.sh
```

## <a name="next-steps"></a><span data-ttu-id="1e1c6-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e1c6-168">Next steps</span></span>

* [<span data-ttu-id="1e1c6-169">Kom igång med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="1e1c6-169">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
