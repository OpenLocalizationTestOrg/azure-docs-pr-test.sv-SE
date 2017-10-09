---
title: "aaaCreate ett Azure Service Fabric tillförlitliga aktörer Java-program på Linux | Microsoft Docs"
description: "Lär dig hur toocreate och distribuera en Java Service Fabric tillförlitliga aktörer program inom fem minuter."
services: service-fabric
documentationcenter: java
author: rwike77
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: ryanwi
ms.openlocfilehash: 11496b767811c89969c65d1682d843448eb6a922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a><span data-ttu-id="92769-103">Skapa ditt första Java Service Fabric Reliable Actors-program på Linux</span><span class="sxs-lookup"><span data-stu-id="92769-103">Create your first Java Service Fabric Reliable Actors application on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="92769-104">C# – Windows</span><span class="sxs-lookup"><span data-stu-id="92769-104">C# - Windows</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
> * [<span data-ttu-id="92769-105">Java – Linux</span><span class="sxs-lookup"><span data-stu-id="92769-105">Java - Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
> * [<span data-ttu-id="92769-106">C# – Linux</span><span class="sxs-lookup"><span data-stu-id="92769-106">C# - Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

<span data-ttu-id="92769-107">Den här snabbstartsguiden hjälper dig att skapa ditt första Azure Service Fabric Java-program i en Linux-utvecklingsmiljö på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="92769-107">This quick-start helps you create your first Azure Service Fabric Java application in a Linux development environment in just a few minutes.</span></span>  <span data-ttu-id="92769-108">När du är klar har du ett enkelt Java tjänsten single program som körs på hello lokal utveckling klustret.</span><span class="sxs-lookup"><span data-stu-id="92769-108">When you are finished, you'll have a simple Java single-service application running on hello local development cluster.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="92769-109">Krav</span><span class="sxs-lookup"><span data-stu-id="92769-109">Prerequisites</span></span>
<span data-ttu-id="92769-110">Innan du börjar installera hello Service Fabric SDK, hello Service Fabric CLI och konfigurera ett kluster för utveckling i din [Linux utvecklingsmiljö](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="92769-110">Before you get started, install hello Service Fabric SDK, hello Service Fabric CLI, and setup a development cluster in your [Linux development environment](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="92769-111">Om du använder Mac OS X kan du [konfigurera en Linux-utvecklingsmiljö på en virtuell dator med hjälp av Vagrant](service-fabric-get-started-mac.md).</span><span class="sxs-lookup"><span data-stu-id="92769-111">If you are using Mac OS X, you can [set up a Linux development environment in a virtual machine using Vagrant](service-fabric-get-started-mac.md).</span></span>

<span data-ttu-id="92769-112">Vill du även tooinstall hello [Service Fabric CLI](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="92769-112">You will also want tooinstall hello [Service Fabric CLI](service-fabric-cli.md).</span></span>

### <a name="install-and-set-up-hello-generators-for-java"></a><span data-ttu-id="92769-113">Installera och konfigurera hello generatorer för Java</span><span class="sxs-lookup"><span data-stu-id="92769-113">Install and set up hello generators for Java</span></span>
<span data-ttu-id="92769-114">Service Fabric tillhandahåller ramverktyg som hjälper dig att skapa ett Service Fabric Java-program från terminalen med en Yeoman-mallgenerator.</span><span class="sxs-lookup"><span data-stu-id="92769-114">Service Fabric provides scaffolding tools which will help you create a Service Fabric Java application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="92769-115">Följ hello stegen nedan tooensure har hello Service Fabric yeoman mall generatorn för Java fungerar på din dator.</span><span class="sxs-lookup"><span data-stu-id="92769-115">Please follow hello steps below tooensure you have hello Service Fabric yeoman template generator for Java working on your machine.</span></span>
1. <span data-ttu-id="92769-116">Installera nodejs och NPM på datorn</span><span class="sxs-lookup"><span data-stu-id="92769-116">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="92769-117">Installera [Yeoman](http://yeoman.io/)-mallgeneratorn på datorn från NPM</span><span class="sxs-lookup"><span data-stu-id="92769-117">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="92769-118">Installera hello Service Fabric Yeo Java-program generator från NPM</span><span class="sxs-lookup"><span data-stu-id="92769-118">Install hello Service Fabric Yeo Java application generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfjava
  ```

## <a name="create-hello-application"></a><span data-ttu-id="92769-119">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="92769-119">Create hello application</span></span>
<span data-ttu-id="92769-120">Ett Service Fabric-program innehåller en eller flera tjänster med en viss roll i att leverera hello-funktionerna.</span><span class="sxs-lookup"><span data-stu-id="92769-120">A Service Fabric application contains one or more services, each with a specific role in delivering hello application's functionality.</span></span> <span data-ttu-id="92769-121">hello generator som du har installerat under hello senaste gör det enkelt toocreate din första tjänst och tooadd mer senare.</span><span class="sxs-lookup"><span data-stu-id="92769-121">hello generator you installed in hello last section, makes it easy toocreate your first service and tooadd more later.</span></span>  <span data-ttu-id="92769-122">Du kan också skapa, bygga och distribuera Service Fabric Java-program med ett plugin-program för Eclipse.</span><span class="sxs-lookup"><span data-stu-id="92769-122">You can also create, build, and deploy Service Fabric Java applications using a plugin for Eclipse.</span></span> <span data-ttu-id="92769-123">Läs mer i [Service Fabric-plugin-program för utveckling av Java-program i Eclipse](service-fabric-get-started-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="92769-123">See [Create and deploy your first Java application using Eclipse](service-fabric-get-started-eclipse.md).</span></span> <span data-ttu-id="92769-124">Använd Yeoman toocreate ett program med en enda tjänst som lagrar och hämtar ett värde för prestandaräknaren för den här snabbstartsguide.</span><span class="sxs-lookup"><span data-stu-id="92769-124">For this quick start, use Yeoman toocreate an application with a single service that stores and gets a counter value.</span></span>

1. <span data-ttu-id="92769-125">I en terminal, skriver du in ``yo azuresfjava``.</span><span class="sxs-lookup"><span data-stu-id="92769-125">In a terminal, type ``yo azuresfjava``.</span></span>
2. <span data-ttu-id="92769-126">Namnge ditt program.</span><span class="sxs-lookup"><span data-stu-id="92769-126">Name your application.</span></span>
3. <span data-ttu-id="92769-127">Välj hello typ av din första tjänst och ge den namnet.</span><span class="sxs-lookup"><span data-stu-id="92769-127">Choose hello type of your first service and name it.</span></span> <span data-ttu-id="92769-128">I den här guiden väljer vi en Reliable Actor-tjänst.</span><span class="sxs-lookup"><span data-stu-id="92769-128">For this tutorial, choose a Reliable Actor Service.</span></span> <span data-ttu-id="92769-129">Mer information om hello finns andra typer av tjänster, [Service Fabric programming översikt över säkerhetsmodell](service-fabric-choose-framework.md).</span><span class="sxs-lookup"><span data-stu-id="92769-129">For more information about hello other types of services, see [Service Fabric programming model overview](service-fabric-choose-framework.md).</span></span>
   <span data-ttu-id="92769-130">![Service Fabric Yeoman-generator för Java][sf-yeoman]</span><span class="sxs-lookup"><span data-stu-id="92769-130">![Service Fabric Yeoman generator for Java][sf-yeoman]</span></span>

## <a name="build-hello-application"></a><span data-ttu-id="92769-131">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="92769-131">Build hello application</span></span>
<span data-ttu-id="92769-132">hello Service Fabric Yeoman mallar innehåller ett build-skript för [Gradle](https://gradle.org/), som du kan använda toobuild hello programmet från hello terminal.</span><span class="sxs-lookup"><span data-stu-id="92769-132">hello Service Fabric Yeoman templates include a build script for [Gradle](https://gradle.org/), which you can use toobuild hello application from hello terminal.</span></span>
<span data-ttu-id="92769-133">Service Fabric Java-beroenden hämtas från Maven.</span><span class="sxs-lookup"><span data-stu-id="92769-133">Service Fabric Java dependencies get fetched from Maven.</span></span> <span data-ttu-id="92769-134">toobuild och arbete på hello Service Fabric Java-program, måste du tooensure att JDK och Gradle är installerade.</span><span class="sxs-lookup"><span data-stu-id="92769-134">toobuild and work on hello Service Fabric Java applications, you need tooensure that you have JDK and Gradle installed.</span></span> <span data-ttu-id="92769-135">Om du ännu inte har installerats kan du köra hello efter tooinstall JDK(openjdk-8-jdk) och Gradle -</span><span class="sxs-lookup"><span data-stu-id="92769-135">If not yet installed, you can run hello following tooinstall JDK(openjdk-8-jdk) and Gradle -</span></span>

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

<span data-ttu-id="92769-136">toobuild och paketet hello program, köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="92769-136">toobuild and package hello application, run hello following:</span></span>

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-hello-application"></a><span data-ttu-id="92769-137">Distribuera programmet hello</span><span class="sxs-lookup"><span data-stu-id="92769-137">Deploy hello application</span></span>
<span data-ttu-id="92769-138">När hello programmet är skapat kan distribuera du den toohello lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="92769-138">Once hello application is built, you can deploy it toohello local cluster.</span></span>

1. <span data-ttu-id="92769-139">Ansluta toohello lokala Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="92769-139">Connect toohello local Service Fabric cluster.</span></span>

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. <span data-ttu-id="92769-140">Kör hello installationsskriptet hello mallen toocopy programmet hello paketet toohello klustret avbildningsarkivet, registrera hello programtyp och skapa en instans av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="92769-140">Run hello install script provided in hello template toocopy hello application package toohello cluster's image store, register hello application type, and create an instance of hello application.</span></span>

    ```bash
    ./install.sh
    ```

<span data-ttu-id="92769-141">Distribuera hello inbyggda program är hello samma som andra Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="92769-141">Deploying hello built application is hello same as any other Service Fabric application.</span></span> <span data-ttu-id="92769-142">Hello-dokumentationen på [hantera ett Service Fabric-program med hello Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) detaljerade anvisningar.</span><span class="sxs-lookup"><span data-stu-id="92769-142">See hello documentation on [managing a Service Fabric application with hello Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) for detailed instructions.</span></span>

<span data-ttu-id="92769-143">Parametrarna toothese kommandon finns i manifest hello genereras inuti hello programpaket.</span><span class="sxs-lookup"><span data-stu-id="92769-143">Parameters toothese commands can be found in hello generated manifests inside hello application package.</span></span>

<span data-ttu-id="92769-144">När programmet hello har distribuerats, öppna en webbläsare och gå till [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) på [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="92769-144">Once hello application has been deployed, open a browser and navigate to [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) at [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span>
<span data-ttu-id="92769-145">Expandera sedan hello **program** nod och Observera att det finns en post för din typ av program och en annan för hello första instansen av den typen.</span><span class="sxs-lookup"><span data-stu-id="92769-145">Then, expand hello **Applications** node and note that there is now an entry for your application type and another for hello first instance of that type.</span></span>

## <a name="start-hello-test-client-and-perform-a-failover"></a><span data-ttu-id="92769-146">Starta hello test-klienten och utför en växling vid fel</span><span class="sxs-lookup"><span data-stu-id="92769-146">Start hello test client and perform a failover</span></span>
<span data-ttu-id="92769-147">Aktörer inte göra någonting på sina egna, de kräver en annan tjänst eller klient toosend dem meddelanden.</span><span class="sxs-lookup"><span data-stu-id="92769-147">Actors do not do anything on their own, they require another service or client toosend them messages.</span></span> <span data-ttu-id="92769-148">hello aktören mallen innehåller ett enkelt testskript som du kan använda toointeract med hello aktören-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="92769-148">hello actor template includes a simple test script that you can use toointeract with hello actor service.</span></span>

1. <span data-ttu-id="92769-149">Köra hello skript med hello titta på verktyget toosee hello utdata från hello aktören service.</span><span class="sxs-lookup"><span data-stu-id="92769-149">Run hello script using hello watch utility toosee hello output of hello actor service.</span></span>  <span data-ttu-id="92769-150">hello test skriptet anropar hello `setCountAsync()` på hello aktören tooincrement en räknare metodanrop hello `getCountAsync()` metoden på hello aktören tooget hello nya räknarvärdet och visar som värdet toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="92769-150">hello test script calls hello `setCountAsync()` method on hello actor tooincrement a counter, calls hello `getCountAsync()` method on hello actor tooget hello new counter value, and displays that value toohello console.</span></span>

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. <span data-ttu-id="92769-151">Leta upp hello nod värd hello primära repliken för hello aktören tjänsten i Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="92769-151">In Service Fabric Explorer, locate hello node hosting hello primary replica for hello actor service.</span></span> <span data-ttu-id="92769-152">I hello skärmbilden nedan är noden 3.</span><span class="sxs-lookup"><span data-stu-id="92769-152">In hello screenshot below, it is node 3.</span></span> <span data-ttu-id="92769-153">hello primära tjänsten replik handtag Läs- och skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="92769-153">hello primary service replica handles read and write operations.</span></span>  <span data-ttu-id="92769-154">Ändringar i tjänstens tillstånd replikeras sedan ut toohello sekundära repliker, som körs på noder 0 och 1 i hello skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="92769-154">Changes in service state are then replicated out toohello secondary replicas, running on nodes 0 and 1 in hello screen shot below.</span></span>

    ![Hitta hello primära repliken i Service Fabric Explorer][sfx-primary]

3. <span data-ttu-id="92769-156">I **noder**, klicka på hello nod du hittades i hello föregående steg och sedan välja **inaktivera (omstart)** hello Åtgärder-menyn.</span><span class="sxs-lookup"><span data-stu-id="92769-156">In **Nodes**, click hello node you found in hello previous step, then select **Deactivate (restart)** from hello Actions menu.</span></span> <span data-ttu-id="92769-157">Den här åtgärden startar om hello-noden som kör hello primära tjänsten replik och tvingar en växling vid fel tooone hello sekundära repliker som körs på en annan nod.</span><span class="sxs-lookup"><span data-stu-id="92769-157">This action restarts hello node running hello primary service replica and forces a failover tooone of hello secondary replicas running on another node.</span></span>  <span data-ttu-id="92769-158">Den sekundära repliken är upphöjt tooprimary, en annan sekundär replik skapas på en annan nod och hello primära repliken börjar tootake läs-/ skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="92769-158">That secondary replica is promoted tooprimary, another secondary replica is created on a different node, and hello primary replica begins tootake read/write operations.</span></span> <span data-ttu-id="92769-159">Hello omstarter av noden, titta på hello utdata från hello testa klient och Observera hello räknaren fortsätter tooincrement trots hello växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="92769-159">As hello node restarts, watch hello output from hello test client and note that hello counter continues tooincrement despite hello failover.</span></span>

## <a name="remove-hello-application"></a><span data-ttu-id="92769-160">Ta bort programmet hello</span><span class="sxs-lookup"><span data-stu-id="92769-160">Remove hello application</span></span>
<span data-ttu-id="92769-161">Använd hello avinstallera skriptet i hello toodelete hello programmet mallinstansen, avregistrera hello programpaket och ta bort hello programpaketet från avbildningsarkivet hello klustret.</span><span class="sxs-lookup"><span data-stu-id="92769-161">Use hello uninstall script provided in hello template toodelete hello application instance, unregister hello application package, and remove hello application package from hello cluster's image store.</span></span>

```bash
./uninstall.sh
```

<span data-ttu-id="92769-162">Du ser att programmet hello och programtyp inte längre visas i hello i Service Fabric explorer **program** nod.</span><span class="sxs-lookup"><span data-stu-id="92769-162">In Service Fabric explorer you see that hello application and application type no longer appear in hello **Applications** node.</span></span>

## <a name="service-fabric-java-libraries-on-maven"></a><span data-ttu-id="92769-163">Service Fabric Java-bibliotek på Maven</span><span class="sxs-lookup"><span data-stu-id="92769-163">Service Fabric Java libraries on Maven</span></span>
<span data-ttu-id="92769-164">Service Fabric Java-bibliotek har lagrats i Maven.</span><span class="sxs-lookup"><span data-stu-id="92769-164">Service Fabric Java libraries have been hosted in Maven.</span></span> <span data-ttu-id="92769-165">Du kan lägga till hello beroenden i hello ``pom.xml`` eller ``build.gradle`` av projekt toouse Service Fabric Java-bibliotek från **mavenCentral**.</span><span class="sxs-lookup"><span data-stu-id="92769-165">You can add hello dependencies in hello ``pom.xml`` or ``build.gradle`` of your projects toouse Service Fabric Java libraries from **mavenCentral**.</span></span>

### <a name="actors"></a><span data-ttu-id="92769-166">Aktörer</span><span class="sxs-lookup"><span data-stu-id="92769-166">Actors</span></span>

<span data-ttu-id="92769-167">Service Fabric-stöd för tillförlitliga aktörer för ditt program.</span><span class="sxs-lookup"><span data-stu-id="92769-167">Service Fabric Reliable Actor support for your application.</span></span>

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-actors-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-actors-preview:0.10.0'
  }
  ```

### <a name="services"></a><span data-ttu-id="92769-168">Tjänster</span><span class="sxs-lookup"><span data-stu-id="92769-168">Services</span></span>

<span data-ttu-id="92769-169">Service Fabric-stöd för tillståndslös tjänst för ditt program.</span><span class="sxs-lookup"><span data-stu-id="92769-169">Service Fabric Stateless Service support for your application.</span></span>

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-services-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-services-preview:0.10.0'
  }
  ```

### <a name="others"></a><span data-ttu-id="92769-170">Andra</span><span class="sxs-lookup"><span data-stu-id="92769-170">Others</span></span>
#### <a name="transport"></a><span data-ttu-id="92769-171">Transport</span><span class="sxs-lookup"><span data-stu-id="92769-171">Transport</span></span>

<span data-ttu-id="92769-172">Transportnivåstöd för Service Fabric Java-program.</span><span class="sxs-lookup"><span data-stu-id="92769-172">Transport layer support for Service Fabric Java application.</span></span> <span data-ttu-id="92769-173">Du behöver inte tooexplicitly lägga till den här beroende tooyour tillförlitliga aktören eller tjänstprogram, såvida inte du programmerar på hello Transportskiktet.</span><span class="sxs-lookup"><span data-stu-id="92769-173">You do not need tooexplicitly add this dependency tooyour Reliable Actor or Service applications, unless you program at hello transport layer.</span></span>

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-transport-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-transport-preview:0.10.0'
  }
  ```

#### <a name="fabric-support"></a><span data-ttu-id="92769-174">Fabric-stöd</span><span class="sxs-lookup"><span data-stu-id="92769-174">Fabric support</span></span>

<span data-ttu-id="92769-175">Nivån stöd för Service Fabric som nämns toonative Service Fabric runtime.</span><span class="sxs-lookup"><span data-stu-id="92769-175">System level support for Service Fabric, which talks toonative Service Fabric runtime.</span></span> <span data-ttu-id="92769-176">Du behöver inte tooexplicitly lägga till det här beroendet tooyour tillförlitliga aktören eller tjänstprogram.</span><span class="sxs-lookup"><span data-stu-id="92769-176">You do not need tooexplicitly add this dependency tooyour Reliable Actor or Service applications.</span></span> <span data-ttu-id="92769-177">Detta hämtar hämtats automatiskt från Maven, när du inkluderar hello andra beroenden som ovan.</span><span class="sxs-lookup"><span data-stu-id="92769-177">This gets fetched automatically from Maven, when you include hello other dependencies above.</span></span>

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-preview:0.10.0'
  }
  ```

## <a name="migrating-old-service-fabric-java-applications-toobe-used-with-maven"></a><span data-ttu-id="92769-178">Migrera gamla Service Fabric Java-program toobe används med Maven</span><span class="sxs-lookup"><span data-stu-id="92769-178">Migrating old Service Fabric Java applications toobe used with Maven</span></span>
<span data-ttu-id="92769-179">Vi har nyligen flyttat Service Fabric Java-bibliotek från Service Fabric Java SDK tooMaven databasen.</span><span class="sxs-lookup"><span data-stu-id="92769-179">We have recently moved Service Fabric Java libraries from Service Fabric Java SDK tooMaven repository.</span></span> <span data-ttu-id="92769-180">Medan hello nya program som du skapar med hjälp av Yeoman eller Eclipse genererar senaste uppdaterade projekt (som kommer att kunna toowork med Maven), kan du uppdatera din befintliga Service Fabric tillståndslös eller aktören Java-program som använder hello Service Fabric Java SDK tidigare toouse hello Service Fabric Java beroenden från Maven.</span><span class="sxs-lookup"><span data-stu-id="92769-180">While hello new applications you generate using Yeoman or Eclipse, will generate latest updated projects (which will be able toowork with Maven), you can update your existing Service Fabric stateless or actor Java applications, which were using hello Service Fabric Java SDK earlier, toouse hello Service Fabric Java dependencies from Maven.</span></span> <span data-ttu-id="92769-181">Följ anvisningarna för hello [här](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure äldre programmet fungerar med Maven.</span><span class="sxs-lookup"><span data-stu-id="92769-181">Please follow hello steps mentioned [here](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure your older application works with Maven.</span></span>

## <a name="next-steps"></a><span data-ttu-id="92769-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="92769-182">Next steps</span></span>

* [<span data-ttu-id="92769-183">Skapa ditt första Service Fabric Java-program för Linux med hjälp av Eclipse</span><span class="sxs-lookup"><span data-stu-id="92769-183">Create your first Service Fabric Java application on Linux using Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="92769-184">Läs mer om Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="92769-184">Learn more about Reliable Actors</span></span>](service-fabric-reliable-actors-introduction.md)
* [<span data-ttu-id="92769-185">Interagera med Service Fabric-kluster med hello Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="92769-185">Interact with Service Fabric clusters using hello Service Fabric CLI</span></span>](service-fabric-cli.md)
* <span data-ttu-id="92769-186">Lär dig mer om [Service Fabric-supportalternativen](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="92769-186">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>
* [<span data-ttu-id="92769-187">Kom igång med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="92769-187">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png
