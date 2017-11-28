---
title: "Skapa ett Azure Service Fabric tillförlitliga aktörer Java-program på Linux | Microsoft Docs"
description: "Lär dig hur du skapar och distribuerar err Java Service Fabric tillförlitliga aktörer-program på fem minuter."
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
ms.openlocfilehash: baf948587ede31fe3d5b4f6f0981269b4cfe4d3d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a><span data-ttu-id="566d4-103">Skapa ditt första Java Service Fabric Reliable Actors-program på Linux</span><span class="sxs-lookup"><span data-stu-id="566d4-103">Create your first Java Service Fabric Reliable Actors application on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="566d4-104">C# – Windows</span><span class="sxs-lookup"><span data-stu-id="566d4-104">C# - Windows</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
> * [<span data-ttu-id="566d4-105">Java – Linux</span><span class="sxs-lookup"><span data-stu-id="566d4-105">Java - Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
> * [<span data-ttu-id="566d4-106">C# – Linux</span><span class="sxs-lookup"><span data-stu-id="566d4-106">C# - Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

<span data-ttu-id="566d4-107">Den här snabbstartsguiden hjälper dig att skapa ditt första Azure Service Fabric Java-program i en Linux-utvecklingsmiljö på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="566d4-107">This quick-start helps you create your first Azure Service Fabric Java application in a Linux development environment in just a few minutes.</span></span>  <span data-ttu-id="566d4-108">När du är klar har du ett enkelt Java-program för en tjänst som körs i klustret för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="566d4-108">When you are finished, you'll have a simple Java single-service application running on the local development cluster.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="566d4-109">Krav</span><span class="sxs-lookup"><span data-stu-id="566d4-109">Prerequisites</span></span>
<span data-ttu-id="566d4-110">Innan du börjar ska du installera Service Fabric SDK, Service Fabric CLI och konfigurera ett utvecklingskluster i [Linux-utvecklingsmiljön](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="566d4-110">Before you get started, install the Service Fabric SDK, the Service Fabric CLI, and setup a development cluster in your [Linux development environment](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="566d4-111">Om du använder Mac OS X kan du [konfigurera en Linux-utvecklingsmiljö på en virtuell dator med hjälp av Vagrant](service-fabric-get-started-mac.md).</span><span class="sxs-lookup"><span data-stu-id="566d4-111">If you are using Mac OS X, you can [set up a Linux development environment in a virtual machine using Vagrant](service-fabric-get-started-mac.md).</span></span>

<span data-ttu-id="566d4-112">Du bör även installera [Service Fabric CLI](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="566d4-112">You will also want to install the [Service Fabric CLI](service-fabric-cli.md).</span></span>

### <a name="install-and-set-up-the-generators-for-java"></a><span data-ttu-id="566d4-113">Installera och konfigurera generatorerna för Java</span><span class="sxs-lookup"><span data-stu-id="566d4-113">Install and set up the generators for Java</span></span>
<span data-ttu-id="566d4-114">Service Fabric tillhandahåller ramverktyg som hjälper dig att skapa ett Service Fabric Java-program från terminalen med en Yeoman-mallgenerator.</span><span class="sxs-lookup"><span data-stu-id="566d4-114">Service Fabric provides scaffolding tools which will help you create a Service Fabric Java application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="566d4-115">Följ stegen nedan för att se till att du har Service Fabric Yeoman-mallgeneratorn för Java på datorn.</span><span class="sxs-lookup"><span data-stu-id="566d4-115">Please follow the steps below to ensure you have the Service Fabric yeoman template generator for Java working on your machine.</span></span>
1. <span data-ttu-id="566d4-116">Installera nodejs och NPM på datorn</span><span class="sxs-lookup"><span data-stu-id="566d4-116">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="566d4-117">Installera [Yeoman](http://yeoman.io/)-mallgeneratorn på datorn från NPM</span><span class="sxs-lookup"><span data-stu-id="566d4-117">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="566d4-118">Installera Service Fabric Yeo Java-programgeneratorn från NPM</span><span class="sxs-lookup"><span data-stu-id="566d4-118">Install the Service Fabric Yeo Java application generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfjava
  ```

## <a name="create-the-application"></a><span data-ttu-id="566d4-119">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="566d4-119">Create the application</span></span>
<span data-ttu-id="566d4-120">Ett Service Fabric-program innehåller en eller flera tjänster, som var och en ansvarar för att leverera programmets funktioner.</span><span class="sxs-lookup"><span data-stu-id="566d4-120">A Service Fabric application contains one or more services, each with a specific role in delivering the application's functionality.</span></span> <span data-ttu-id="566d4-121">Generatorn som du installerade i förra avsnittet gör det enkelt att skapa din första tjänst och lägga till fler senare.</span><span class="sxs-lookup"><span data-stu-id="566d4-121">The generator you installed in the last section, makes it easy to create your first service and to add more later.</span></span>  <span data-ttu-id="566d4-122">Du kan också skapa, bygga och distribuera Service Fabric Java-program med ett plugin-program för Eclipse.</span><span class="sxs-lookup"><span data-stu-id="566d4-122">You can also create, build, and deploy Service Fabric Java applications using a plugin for Eclipse.</span></span> <span data-ttu-id="566d4-123">Läs mer i [Service Fabric-plugin-program för utveckling av Java-program i Eclipse](service-fabric-get-started-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="566d4-123">See [Create and deploy your first Java application using Eclipse](service-fabric-get-started-eclipse.md).</span></span> <span data-ttu-id="566d4-124">I den här snabbstartguiden använder vi Yeoman till att skapa ett program med en enda tjänst, som lagrar och hämtar ett värde.</span><span class="sxs-lookup"><span data-stu-id="566d4-124">For this quick start, use Yeoman to create an application with a single service that stores and gets a counter value.</span></span>

1. <span data-ttu-id="566d4-125">I en terminal, skriver du in ``yo azuresfjava``.</span><span class="sxs-lookup"><span data-stu-id="566d4-125">In a terminal, type ``yo azuresfjava``.</span></span>
2. <span data-ttu-id="566d4-126">Namnge ditt program.</span><span class="sxs-lookup"><span data-stu-id="566d4-126">Name your application.</span></span>
3. <span data-ttu-id="566d4-127">Välj vilken typ din första tjänst ska ha och namnge den.</span><span class="sxs-lookup"><span data-stu-id="566d4-127">Choose the type of your first service and name it.</span></span> <span data-ttu-id="566d4-128">I den här guiden väljer vi en Reliable Actor-tjänst.</span><span class="sxs-lookup"><span data-stu-id="566d4-128">For this tutorial, choose a Reliable Actor Service.</span></span> <span data-ttu-id="566d4-129">Mer information om andra typer av tjänster finns i [Service Fabric programming model overview](service-fabric-choose-framework.md) (Översikt över programmeringsmodellen i Service Fabric).</span><span class="sxs-lookup"><span data-stu-id="566d4-129">For more information about the other types of services, see [Service Fabric programming model overview](service-fabric-choose-framework.md).</span></span>
   <span data-ttu-id="566d4-130">![Service Fabric Yeoman-generator för Java][sf-yeoman]</span><span class="sxs-lookup"><span data-stu-id="566d4-130">![Service Fabric Yeoman generator for Java][sf-yeoman]</span></span>

## <a name="build-the-application"></a><span data-ttu-id="566d4-131">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="566d4-131">Build the application</span></span>
<span data-ttu-id="566d4-132">I Service Fabric Yeoman-mallarna ingår ett byggskript för [Gradle](https://gradle.org/) som du kan använda för att skapa programmet från terminalen.</span><span class="sxs-lookup"><span data-stu-id="566d4-132">The Service Fabric Yeoman templates include a build script for [Gradle](https://gradle.org/), which you can use to build the application from the terminal.</span></span>
<span data-ttu-id="566d4-133">Service Fabric Java-beroenden hämtas från Maven.</span><span class="sxs-lookup"><span data-stu-id="566d4-133">Service Fabric Java dependencies get fetched from Maven.</span></span> <span data-ttu-id="566d4-134">Om du vill skapa och arbeta med Service Fabric Java-programmen måste du se till att du har JDK och Gradle installerade.</span><span class="sxs-lookup"><span data-stu-id="566d4-134">To build and work on the Service Fabric Java applications, you need to ensure that you have JDK and Gradle installed.</span></span> <span data-ttu-id="566d4-135">Om de inte har installerats än kan du köra följande för att installera JDK (openjdk-8-jdk) och Gradle -</span><span class="sxs-lookup"><span data-stu-id="566d4-135">If not yet installed, you can run the following to install JDK(openjdk-8-jdk) and Gradle -</span></span>

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

<span data-ttu-id="566d4-136">När du ska bygga och paketera programmet kör du följande:</span><span class="sxs-lookup"><span data-stu-id="566d4-136">To build and package the application, run the following:</span></span>

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a><span data-ttu-id="566d4-137">Distribuera programmet</span><span class="sxs-lookup"><span data-stu-id="566d4-137">Deploy the application</span></span>
<span data-ttu-id="566d4-138">När du har skapat programmet kan du distribuera det till det lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="566d4-138">Once the application is built, you can deploy it to the local cluster.</span></span>

1. <span data-ttu-id="566d4-139">Anslut till det lokala Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="566d4-139">Connect to the local Service Fabric cluster.</span></span>

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. <span data-ttu-id="566d4-140">Kör installationsskriptet som medföljer mallen för att kopiera programpaketet till klustrets avbildningsarkiv, registrera programtypen och skapa en instans av programmet.</span><span class="sxs-lookup"><span data-stu-id="566d4-140">Run the install script provided in the template to copy the application package to the cluster's image store, register the application type, and create an instance of the application.</span></span>

    ```bash
    ./install.sh
    ```

<span data-ttu-id="566d4-141">Distributionen går till på samma sätt som för andra Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="566d4-141">Deploying the built application is the same as any other Service Fabric application.</span></span> <span data-ttu-id="566d4-142">Detaljerade instruktioner finns i dokumentationen om att [hantera ett Service Fabric-program med Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md).</span><span class="sxs-lookup"><span data-stu-id="566d4-142">See the documentation on [managing a Service Fabric application with the Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) for detailed instructions.</span></span>

<span data-ttu-id="566d4-143">Du hittar parametrarna till de här kommandona i de genererade manifesten i programpaketet.</span><span class="sxs-lookup"><span data-stu-id="566d4-143">Parameters to these commands can be found in the generated manifests inside the application package.</span></span>

<span data-ttu-id="566d4-144">När programmet har distribuerats öppnar du en webbläsare och går till [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) på adressen [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="566d4-144">Once the application has been deployed, open a browser and navigate to [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) at [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span>
<span data-ttu-id="566d4-145">Expandera sedan noden **Program** och observera att det nu finns en post för din programtyp och en post för den första instansen av den typen.</span><span class="sxs-lookup"><span data-stu-id="566d4-145">Then, expand the **Applications** node and note that there is now an entry for your application type and another for the first instance of that type.</span></span>

## <a name="start-the-test-client-and-perform-a-failover"></a><span data-ttu-id="566d4-146">Starta testklienten och utför en redundansväxling</span><span class="sxs-lookup"><span data-stu-id="566d4-146">Start the test client and perform a failover</span></span>
<span data-ttu-id="566d4-147">Aktörer gör ingenting på egen hand, det behövs en annan tjänst eller klient för att skicka meddelanden till dem.</span><span class="sxs-lookup"><span data-stu-id="566d4-147">Actors do not do anything on their own, they require another service or client to send them messages.</span></span> <span data-ttu-id="566d4-148">Aktörsmallen innehåller ett enkelt testskript som du kan använda för att interagera med aktörstjänsten.</span><span class="sxs-lookup"><span data-stu-id="566d4-148">The actor template includes a simple test script that you can use to interact with the actor service.</span></span>

1. <span data-ttu-id="566d4-149">Kör skriptet med övervakningsverktyget för att se resultatet av aktörstjänsten.</span><span class="sxs-lookup"><span data-stu-id="566d4-149">Run the script using the watch utility to see the output of the actor service.</span></span>  <span data-ttu-id="566d4-150">Testskriptet anropar metoden `setCountAsync()` hos aktören för att öka en räknare, anropar metoden `getCountAsync()` hos aktören för att hämta det nya räknarvärdet och visar värdet på konsolen.</span><span class="sxs-lookup"><span data-stu-id="566d4-150">The test script calls the `setCountAsync()` method on the actor to increment a counter, calls the `getCountAsync()` method on the actor to get the new counter value, and displays that value to the console.</span></span>

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. <span data-ttu-id="566d4-151">Leta rätt på noden i Service Fabric Explorer som är värd för aktörstjänstens primära replik.</span><span class="sxs-lookup"><span data-stu-id="566d4-151">In Service Fabric Explorer, locate the node hosting the primary replica for the actor service.</span></span> <span data-ttu-id="566d4-152">På skärmbilden nedan är det nod 3.</span><span class="sxs-lookup"><span data-stu-id="566d4-152">In the screenshot below, it is node 3.</span></span> <span data-ttu-id="566d4-153">Den primära tjänsterepliken hanterar läs- och skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="566d4-153">The primary service replica handles read and write operations.</span></span>  <span data-ttu-id="566d4-154">Ändringar i tjänstens tillstånd replikeras sedan ut till de sekundära replikerna som körs på noderna 0 och 1 i skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="566d4-154">Changes in service state are then replicated out to the secondary replicas, running on nodes 0 and 1 in the screen shot below.</span></span>

    ![Hitta den primära repliken i Service Fabric Explorer][sfx-primary]

3. <span data-ttu-id="566d4-156">Klicka på noden du hittade i föregående steg under **Noder** och välj sedan **Inaktivera (starta om)** på menyn Åtgärder.</span><span class="sxs-lookup"><span data-stu-id="566d4-156">In **Nodes**, click the node you found in the previous step, then select **Deactivate (restart)** from the Actions menu.</span></span> <span data-ttu-id="566d4-157">Den här åtgärden startar om noden som kör den primära tjänsterepliken och tvingar fram en redundansväxling till en av de sekundära replikerna som körs på en annan nod.</span><span class="sxs-lookup"><span data-stu-id="566d4-157">This action restarts the node running the primary service replica and forces a failover to one of the secondary replicas running on another node.</span></span>  <span data-ttu-id="566d4-158">Den sekundära repliken befordras till primär, en annan sekundär replik skapas på en annan nod och den primära repliken börjar hantera läs- och skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="566d4-158">That secondary replica is promoted to primary, another secondary replica is created on a different node, and the primary replica begins to take read/write operations.</span></span> <span data-ttu-id="566d4-159">Titta på resultatet från testklienten när noden startas om, och observera att räknaren fortsätter att räkna upp trots redundansväxlingen.</span><span class="sxs-lookup"><span data-stu-id="566d4-159">As the node restarts, watch the output from the test client and note that the counter continues to increment despite the failover.</span></span>

## <a name="remove-the-application"></a><span data-ttu-id="566d4-160">Ta bort programmet</span><span class="sxs-lookup"><span data-stu-id="566d4-160">Remove the application</span></span>
<span data-ttu-id="566d4-161">Använd installationsskriptet som medföljer mallen för att ta bort programinstansen, avregistrera programpaketet och ta bort programpaketet från klustrets avbildningslager.</span><span class="sxs-lookup"><span data-stu-id="566d4-161">Use the uninstall script provided in the template to delete the application instance, unregister the application package, and remove the application package from the cluster's image store.</span></span>

```bash
./uninstall.sh
```

<span data-ttu-id="566d4-162">I Service Fabric Explorer ser du att programmet och programtypen inte längre visas i noden **Program**.</span><span class="sxs-lookup"><span data-stu-id="566d4-162">In Service Fabric explorer you see that the application and application type no longer appear in the **Applications** node.</span></span>

## <a name="service-fabric-java-libraries-on-maven"></a><span data-ttu-id="566d4-163">Service Fabric Java-bibliotek på Maven</span><span class="sxs-lookup"><span data-stu-id="566d4-163">Service Fabric Java libraries on Maven</span></span>
<span data-ttu-id="566d4-164">Service Fabric Java-bibliotek har lagrats i Maven.</span><span class="sxs-lookup"><span data-stu-id="566d4-164">Service Fabric Java libraries have been hosted in Maven.</span></span> <span data-ttu-id="566d4-165">Du kan lägga till beroendena i ``pom.xml`` eller ``build.gradle`` för dina projekt så att de använder Service Fabric Java-bibliotek från **mavenCentral**.</span><span class="sxs-lookup"><span data-stu-id="566d4-165">You can add the dependencies in the ``pom.xml`` or ``build.gradle`` of your projects to use Service Fabric Java libraries from **mavenCentral**.</span></span>

### <a name="actors"></a><span data-ttu-id="566d4-166">Aktörer</span><span class="sxs-lookup"><span data-stu-id="566d4-166">Actors</span></span>

<span data-ttu-id="566d4-167">Service Fabric-stöd för tillförlitliga aktörer för ditt program.</span><span class="sxs-lookup"><span data-stu-id="566d4-167">Service Fabric Reliable Actor support for your application.</span></span>

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

### <a name="services"></a><span data-ttu-id="566d4-168">Tjänster</span><span class="sxs-lookup"><span data-stu-id="566d4-168">Services</span></span>

<span data-ttu-id="566d4-169">Service Fabric-stöd för tillståndslös tjänst för ditt program.</span><span class="sxs-lookup"><span data-stu-id="566d4-169">Service Fabric Stateless Service support for your application.</span></span>

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

### <a name="others"></a><span data-ttu-id="566d4-170">Andra</span><span class="sxs-lookup"><span data-stu-id="566d4-170">Others</span></span>
#### <a name="transport"></a><span data-ttu-id="566d4-171">Transport</span><span class="sxs-lookup"><span data-stu-id="566d4-171">Transport</span></span>

<span data-ttu-id="566d4-172">Transportnivåstöd för Service Fabric Java-program.</span><span class="sxs-lookup"><span data-stu-id="566d4-172">Transport layer support for Service Fabric Java application.</span></span> <span data-ttu-id="566d4-173">Du behöver inte uttryckligen lägga till det här beroendet till tillförlitliga aktörer- eller tjänstprogram, om du inte programmerar på transportnivån.</span><span class="sxs-lookup"><span data-stu-id="566d4-173">You do not need to explicitly add this dependency to your Reliable Actor or Service applications, unless you program at the transport layer.</span></span>

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

#### <a name="fabric-support"></a><span data-ttu-id="566d4-174">Fabric-stöd</span><span class="sxs-lookup"><span data-stu-id="566d4-174">Fabric support</span></span>

<span data-ttu-id="566d4-175">Systemnivåstöd för Service Fabric, som kommunicerar med ursprunglig Service Fabric-körning.</span><span class="sxs-lookup"><span data-stu-id="566d4-175">System level support for Service Fabric, which talks to native Service Fabric runtime.</span></span> <span data-ttu-id="566d4-176">Du behöver inte uttryckligen lägga till det här beroendet till tillförlitliga aktörer- eller tjänstprogram.</span><span class="sxs-lookup"><span data-stu-id="566d4-176">You do not need to explicitly add this dependency to your Reliable Actor or Service applications.</span></span> <span data-ttu-id="566d4-177">Det hämtas automatiskt från Maven när du inkluderar de andra beroendena ovan.</span><span class="sxs-lookup"><span data-stu-id="566d4-177">This gets fetched automatically from Maven, when you include the other dependencies above.</span></span>

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

## <a name="migrating-old-service-fabric-java-applications-to-be-used-with-maven"></a><span data-ttu-id="566d4-178">Migrera gamla Service Fabric Java-program som ska användas med Maven</span><span class="sxs-lookup"><span data-stu-id="566d4-178">Migrating old Service Fabric Java applications to be used with Maven</span></span>
<span data-ttu-id="566d4-179">Vi har nyligen flyttat Service Fabric Java-bibliotek från Service Fabric Java-SDK till Maven-centrallager.</span><span class="sxs-lookup"><span data-stu-id="566d4-179">We have recently moved Service Fabric Java libraries from Service Fabric Java SDK to Maven repository.</span></span> <span data-ttu-id="566d4-180">De nya program som du genererar med hjälp av Yeoman eller Eclipse genererar senast uppdaterade projekt (som fungerar med Maven), men du kan uppdatera dina befintliga Service Fabric Java tillståndslösa eller aktörsprogram, som tidigare använde Service Fabric Java SDK, för att använda Service Fabric Java-beroenden från Maven.</span><span class="sxs-lookup"><span data-stu-id="566d4-180">While the new applications you generate using Yeoman or Eclipse, will generate latest updated projects (which will be able to work with Maven), you can update your existing Service Fabric stateless or actor Java applications, which were using the Service Fabric Java SDK earlier, to use the Service Fabric Java dependencies from Maven.</span></span> <span data-ttu-id="566d4-181">Följ anvisningarna [här](service-fabric-migrate-old-javaapp-to-use-maven.md) för att se till att det äldre programmet fungerar med Maven.</span><span class="sxs-lookup"><span data-stu-id="566d4-181">Please follow the steps mentioned [here](service-fabric-migrate-old-javaapp-to-use-maven.md) to ensure your older application works with Maven.</span></span>

## <a name="next-steps"></a><span data-ttu-id="566d4-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="566d4-182">Next steps</span></span>

* [<span data-ttu-id="566d4-183">Skapa ditt första Service Fabric Java-program för Linux med hjälp av Eclipse</span><span class="sxs-lookup"><span data-stu-id="566d4-183">Create your first Service Fabric Java application on Linux using Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="566d4-184">Läs mer om Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="566d4-184">Learn more about Reliable Actors</span></span>](service-fabric-reliable-actors-introduction.md)
* [<span data-ttu-id="566d4-185">Interagera med Service Fabric-kluster med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="566d4-185">Interact with Service Fabric clusters using the Service Fabric CLI</span></span>](service-fabric-cli.md)
* <span data-ttu-id="566d4-186">Lär dig mer om [Service Fabric-supportalternativen](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="566d4-186">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>
* [<span data-ttu-id="566d4-187">Kom igång med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="566d4-187">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png
