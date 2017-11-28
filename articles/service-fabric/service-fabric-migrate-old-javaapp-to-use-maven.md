---
title: "Migrera från Java SDK till Maven – uppdatera gamla Azure Service Fabric Java-program för att använda Maven | Microsoft Docs"
description: "Uppdatera de äldre Java-programmen som använde Service Fabric Java-SDK:n för att hämta Service Fabric Java-beroenden från Maven. När du har slutfört konfigurationen kan de äldre Java-programmen skapa."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: saysa
ms.openlocfilehash: 2123c5f26d77045bd22af56a844fdbf222930e7b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="update-your-previous-java-service-fabric-application-to-fetch-java-libraries-from-maven"></a><span data-ttu-id="447bd-104">Uppdatera det tidigare Service Fabric Java-programmet för att hämta Java-bibliotek från Maven</span><span class="sxs-lookup"><span data-stu-id="447bd-104">Update your previous Java Service Fabric application to fetch Java libraries from Maven</span></span>
<span data-ttu-id="447bd-105">Vi har nyligen flyttat Service Fabric Java-binärfiler från Service Fabric Java-SDK:n till Maven-lagring.</span><span class="sxs-lookup"><span data-stu-id="447bd-105">We have recently moved Service Fabric Java binaries from the Service Fabric Java SDK to Maven hosting.</span></span> <span data-ttu-id="447bd-106">Nu kan du använda **mavencentral** för att hämta de senaste Service Fabric Java-beroendena.</span><span class="sxs-lookup"><span data-stu-id="447bd-106">Now you can use **mavencentral** to fetch the latest Service Fabric Java dependencies.</span></span> <span data-ttu-id="447bd-107">I den här snabbstarten får du hjälp att uppdatera dina befintliga Java-program, som du tidigare har skapat för användning med Service Fabric Java-SDK:n, med hjälp av en Yeoman-mall eller Eclipse, för att vara kompatibla med den Maven-baserade versionen.</span><span class="sxs-lookup"><span data-stu-id="447bd-107">This quick-start helps you update your existing Java applications, which you earlier created to be used with Service Fabric Java SDK, using either Yeoman template or Eclipse, to be compatible with the Maven based build.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="447bd-108">Krav</span><span class="sxs-lookup"><span data-stu-id="447bd-108">Prerequisites</span></span>
1. <span data-ttu-id="447bd-109">Först måste du avinstallera den befintliga Java-SDK:n.</span><span class="sxs-lookup"><span data-stu-id="447bd-109">First you need to uninstall the existing Java SDK.</span></span>

  ```bash
  sudo dpkg -r servicefabricsdkjava
  ```
2. <span data-ttu-id="447bd-110">Installera senaste Service Fabric CLI enligt instruktionerna [här](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="447bd-110">Install the latest Service Fabric CLI following the steps mentioned [here](service-fabric-cli.md).</span></span>

3. <span data-ttu-id="447bd-111">Om du vill skapa och arbeta med Service Fabric Java-programmen måste du se till att du har JDK 1.8 och Gradle installerade.</span><span class="sxs-lookup"><span data-stu-id="447bd-111">To build and work on the Service Fabric Java applications, you need to ensure that you have JDK 1.8 and Gradle installed.</span></span> <span data-ttu-id="447bd-112">Om de inte har installerats än kan du köra följande för att installera JDK 1.8 (openjdk-8-jdk) och Gradle -</span><span class="sxs-lookup"><span data-stu-id="447bd-112">If not yet installed, you can run the following to install JDK 1.8 (openjdk-8-jdk) and Gradle -</span></span>

 ```bash
 sudo apt-get install openjdk-8-jdk-headless
 sudo apt-get install gradle
 ```
4. <span data-ttu-id="447bd-113">Uppdatera installations-/avinstallationsskripten för programmet så att de använder nya Service Fabric CLI enligt instruktionerna [här](service-fabric-application-lifecycle-sfctl.md).</span><span class="sxs-lookup"><span data-stu-id="447bd-113">Update the install/uninstall scripts of your application to use the new Service Fabric CLI following the steps mentioned [here](service-fabric-application-lifecycle-sfctl.md).</span></span> <span data-ttu-id="447bd-114">Du kan använda våra komma igång-[exempel](https://github.com/Azure-Samples/service-fabric-java-getting-started) som referens.</span><span class="sxs-lookup"><span data-stu-id="447bd-114">You can refer to our getting-started [examples](https://github.com/Azure-Samples/service-fabric-java-getting-started) for reference.</span></span>

>[!TIP]
> <span data-ttu-id="447bd-115">När du har avinstallerat Service Fabric Java-SDK:n fungerar inte Yeoman.</span><span class="sxs-lookup"><span data-stu-id="447bd-115">After uninstalling the Service Fabric Java SDK, Yeoman will not work.</span></span> <span data-ttu-id="447bd-116">Följ kraven [här](service-fabric-create-your-first-linux-application-with-java.md) för att få igång Service Fabric Yeoman Java-mallgeneratorn.</span><span class="sxs-lookup"><span data-stu-id="447bd-116">Follow the Prerequisites mentioned [here](service-fabric-create-your-first-linux-application-with-java.md) to have Service Fabric Yeoman Java template generator up and working.</span></span>

## <a name="service-fabric-java-libraries-on-maven"></a><span data-ttu-id="447bd-117">Service Fabric Java-bibliotek på Maven</span><span class="sxs-lookup"><span data-stu-id="447bd-117">Service Fabric Java libraries on Maven</span></span>
<span data-ttu-id="447bd-118">Service Fabric Java-bibliotek har lagrats i Maven.</span><span class="sxs-lookup"><span data-stu-id="447bd-118">Service Fabric Java libraries have been hosted in Maven.</span></span> <span data-ttu-id="447bd-119">Du kan lägga till beroendena i ``pom.xml`` eller ``build.gradle`` för dina projekt så att de använder Service Fabric Java-bibliotek från **mavenCentral**.</span><span class="sxs-lookup"><span data-stu-id="447bd-119">You can add the dependencies in the ``pom.xml`` or ``build.gradle`` of your projects to use Service Fabric Java libraries from **mavenCentral**.</span></span>

### <a name="actors"></a><span data-ttu-id="447bd-120">Aktörer</span><span class="sxs-lookup"><span data-stu-id="447bd-120">Actors</span></span>

<span data-ttu-id="447bd-121">Service Fabric-stöd för tillförlitliga aktörer för ditt program.</span><span class="sxs-lookup"><span data-stu-id="447bd-121">Service Fabric Reliable Actor support for your application.</span></span>

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

### <a name="services"></a><span data-ttu-id="447bd-122">Tjänster</span><span class="sxs-lookup"><span data-stu-id="447bd-122">Services</span></span>

<span data-ttu-id="447bd-123">Service Fabric-stöd för tillståndslös tjänst för ditt program.</span><span class="sxs-lookup"><span data-stu-id="447bd-123">Service Fabric Stateless Service support for your application.</span></span>

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

### <a name="others"></a><span data-ttu-id="447bd-124">Andra</span><span class="sxs-lookup"><span data-stu-id="447bd-124">Others</span></span>
#### <a name="transport"></a><span data-ttu-id="447bd-125">Transport</span><span class="sxs-lookup"><span data-stu-id="447bd-125">Transport</span></span>

<span data-ttu-id="447bd-126">Transportnivåstöd för Service Fabric Java-program.</span><span class="sxs-lookup"><span data-stu-id="447bd-126">Transport layer support for Service Fabric Java application.</span></span> <span data-ttu-id="447bd-127">Du behöver inte uttryckligen lägga till det här beroendet till tillförlitliga aktörer- eller tjänstprogram, om du inte programmerar på transportnivån.</span><span class="sxs-lookup"><span data-stu-id="447bd-127">You do not need to explicitly add this dependency to your Reliable Actor or Service applications, unless you program at the transport layer.</span></span>

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

#### <a name="fabric-support"></a><span data-ttu-id="447bd-128">Fabric-stöd</span><span class="sxs-lookup"><span data-stu-id="447bd-128">Fabric support</span></span>

<span data-ttu-id="447bd-129">Systemnivåstöd för Service Fabric, som kommunicerar med ursprunglig Service Fabric-körning.</span><span class="sxs-lookup"><span data-stu-id="447bd-129">System level support for Service Fabric, which talks to native Service Fabric runtime.</span></span> <span data-ttu-id="447bd-130">Du behöver inte uttryckligen lägga till det här beroendet till tillförlitliga aktörer- eller tjänstprogram.</span><span class="sxs-lookup"><span data-stu-id="447bd-130">You do not need to explicitly add this dependency to your Reliable Actor or Service applications.</span></span> <span data-ttu-id="447bd-131">Det hämtas automatiskt från Maven när du inkluderar de andra beroendena ovan.</span><span class="sxs-lookup"><span data-stu-id="447bd-131">This gets fetched automatically from Maven, when you include the other dependencies above.</span></span>

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


## <a name="migrating-service-fabric-stateless-service"></a><span data-ttu-id="447bd-132">Migrera tillståndslös Service Fabric-tjänst</span><span class="sxs-lookup"><span data-stu-id="447bd-132">Migrating Service Fabric Stateless Service</span></span>

<span data-ttu-id="447bd-133">Om du vill kunna skapa en befintlig tillståndslös Service Fabric Java-tjänst med hjälp av Service Fabric-beroenden som hämtas från Maven, måste du uppdatera filen ``build.gradle`` i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="447bd-133">To be able to build your existing Service Fabric stateless Java service using Service Fabric dependencies fetched from Maven, you need to update the ``build.gradle`` file inside the Service.</span></span> <span data-ttu-id="447bd-134">Tidigare såg det ut så här -</span><span class="sxs-lookup"><span data-stu-id="447bd-134">Previously it used to be like as follows -</span></span>
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
    compile project(':Interface')
}
.
.
.
jar {
    manifest {
    attributes(
                'Main-Class': 'statelessservice.MyStatelessServiceHost',
                "Class-Path": configurations.compile.collect { 'lib/' + it.getName() }.join(' '))
    baseName "MyStateless"
    destinationDir = file('./../MyStatelessApplication/MyStatelessPkg/Code')
}
.
.
.
task copyDeps <<{
    copy {
        from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
        into("./../MyStatelessApplication/MyStatelessPkg/Code/lib")
        include('*.jar')
    }
    copy {
        from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
        into("./../MyStatelessApplication/MyStatelessPkg/Code/lib")
        include('libj*.so')
    }
}
```
<span data-ttu-id="447bd-135">För att hämta beroendena från Maven skulle **uppdaterade** ``build.gradle`` nu ha motsvarande delar enligt följande -</span><span class="sxs-lookup"><span data-stu-id="447bd-135">Now, to fetch the dependencies from Maven, the **updated** ``build.gradle`` would have the corresponding parts as follows -</span></span>
```
repositories {
        mavenCentral()
}

configurations {
    azuresf
}

dependencies {
    compile project(':Interface')
    azuresf ('com.microsoft.servicefabric:sf-services-preview:0.10.0')
    compile fileTree(dir: 'lib', include: '*.jar')
}

task explodeDeps(type: Copy, dependsOn:configurations.azuresf) { task ->
    configurations.azuresf.filter { it.toString().contains("native-preview") }.each{
        from zipTree(it)
    }
    configurations.azuresf.filter { !it.toString().contains("native-preview") }.each {
        from it
    }
    into "lib"
    include "libj*.so", "*.jar"
}

compileJava.dependsOn(explodeDeps)
.
.
.
jar {
    manifest {
        def mpath = configurations.compile.collect {'lib/'+it.getName()}.join (' ')
        mpath = mpath + ' ' + configurations.azuresf.collect {'lib/'+it.getName()}.join (' ')
        attributes(
                'Main-Class': 'statelessservice.MyStatelessServiceHost',
                "Class-Path": mpath)
    baseName "MyStateless"
    destinationDir = file('./../MyStatelessApplication/MyStatelessPkg/Code')
   }
}
.
.
.
task copyDeps <<{
    copy {
        from("lib/")
        into("./../MyStatelessApplication/MyStatelessPkg/Code/lib")
        include('*')
    }
}
```
<span data-ttu-id="447bd-136">I allmänhet kan du titta på något av våra komma igång-exempel för att få en uppfattning om hur byggskriptet skulle se ut för en tillståndslös Service Fabric Java-tjänst.</span><span class="sxs-lookup"><span data-stu-id="447bd-136">In general, to get an overall idea about how the build script would look like for a Service Fabric stateless Java service, you can refer to any sample from our getting-started examples.</span></span> <span data-ttu-id="447bd-137">Här är [build.gradle](https://github.com/Azure-Samples/service-fabric-java-getting-started/blob/master/Services/EchoServer/EchoServer1.0/EchoServerService/build.gradle) för EchoServer-exemplet.</span><span class="sxs-lookup"><span data-stu-id="447bd-137">Here is the [build.gradle](https://github.com/Azure-Samples/service-fabric-java-getting-started/blob/master/Services/EchoServer/EchoServer1.0/EchoServerService/build.gradle) for the EchoServer sample.</span></span>

## <a name="migrating-service-fabric-actor-service"></a><span data-ttu-id="447bd-138">Migrera Service Fabric-aktörstjänst</span><span class="sxs-lookup"><span data-stu-id="447bd-138">Migrating Service Fabric Actor Service</span></span>

<span data-ttu-id="447bd-139">Om du vill kunna skapa ett befintligt Service Fabric Java-aktörsprogram med hjälp av Service Fabric-beroenden som hämtas från Maven, måste du uppdatera filen ``build.gradle`` i gränssnittspaketet och i tjänstpaketet.</span><span class="sxs-lookup"><span data-stu-id="447bd-139">To be able to build your existing Service Fabric Actor Java application using Service Fabric dependencies fetched from Maven, you need to update the ``build.gradle`` file inside the interface package and in the Service package.</span></span> <span data-ttu-id="447bd-140">Om du har ett TestClient-paket måste du uppdatera det också.</span><span class="sxs-lookup"><span data-stu-id="447bd-140">If you have a TestClient package, you need to update that as well.</span></span> <span data-ttu-id="447bd-141">Så för aktören ``Myactor`` behöver du uppdatera på följande platser -</span><span class="sxs-lookup"><span data-stu-id="447bd-141">So, for your actor ``Myactor``, the following would be the places where you need to update -</span></span>
```
./Myactor/build.gradle
./MyactorInterface/build.gradle
./MyactorTestClient/build.gradle
```

#### <a name="updating-build-script-for-the-interface-project"></a><span data-ttu-id="447bd-142">Uppdatera byggskript för gränssnittsprojektet</span><span class="sxs-lookup"><span data-stu-id="447bd-142">Updating build script for the interface project</span></span>

<span data-ttu-id="447bd-143">Tidigare såg det ut så här -</span><span class="sxs-lookup"><span data-stu-id="447bd-143">Previously it used to be like as follows -</span></span>
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
}
.
.
```
<span data-ttu-id="447bd-144">För att hämta beroendena från Maven skulle **uppdaterade** ``build.gradle`` nu ha motsvarande delar enligt följande -</span><span class="sxs-lookup"><span data-stu-id="447bd-144">Now, to fetch the dependencies from Maven, the **updated** ``build.gradle`` would have the corresponding parts as follows -</span></span>
```
repositories {
    mavenCentral()
}

configurations {
    azuresf
}

dependencies {
    azuresf ('com.microsoft.servicefabric:sf-actors-preview:0.10.0')
    compile fileTree(dir: 'lib', include: '*.jar')
}

task explodeDeps(type: Copy, dependsOn:configurations.azuresf) { task ->
    configurations.azuresf.filter { it.toString().contains("native-preview") }.each{
        from zipTree(it)
    }
    configurations.azuresf.filter { !it.toString().contains("native-preview") }.each {
        from it
    }
    into "lib"
    include "libj*.so", "*.jar"
}

compileJava.dependsOn(explodeDeps)
.
.
```

#### <a name="updating-build-script-for-the-actor-project"></a><span data-ttu-id="447bd-145">Uppdatera byggskript för aktörsprojektet</span><span class="sxs-lookup"><span data-stu-id="447bd-145">Updating build script for the actor project</span></span>

<span data-ttu-id="447bd-146">Tidigare såg det ut så här -</span><span class="sxs-lookup"><span data-stu-id="447bd-146">Previously it used to be like as follows -</span></span>
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
    compile project(':MyactorInterface')
}
.
.
.
jar {
    manifest {
    attributes(
                'Main-Class': 'reliableactor.MyactorHost',
                "Class-Path": configurations.compile.collect { 'lib/' + it.getName() }.join(' '))
      baseName "myactor"
    destinationDir = file('./../myjavaapp/MyactorPkg/Code')
    }
}
.
.
.
task copyDeps<< {
    copy {
        from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
        into("./../myjavaapp/MyactorPkg/Code/lib")
        include('*.jar')
    }
    copy {
        from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
        into("./../myjavaapp/MyactorPkg/Code/lib")
        include('libj*.so')
    }
    copy {
        from("../MyactorInterface/out/lib")
        into("./../myjavaapp/MyactorPkg/Code/lib")
        include('*.jar')
    }
}
```
<span data-ttu-id="447bd-147">För att hämta beroendena från Maven skulle **uppdaterade** ``build.gradle`` nu ha motsvarande delar enligt följande -</span><span class="sxs-lookup"><span data-stu-id="447bd-147">Now, to fetch the dependencies from Maven, the **updated** ``build.gradle`` would have the corresponding parts as follows -</span></span>
```
repositories {
    mavenCentral()
}

configurations {
    azuresf
}

dependencies {
    compile project(':MyactorInterface')
    azuresf ('com.microsoft.servicefabric:sf-actors-preview:0.10.0')
    compile fileTree(dir: 'lib', include: '*.jar')
}

task explodeDeps(type: Copy, dependsOn:configurations.azuresf) { task ->
    configurations.azuresf.filter { it.toString().contains("native-preview") }.each{
        from zipTree(it)
    }
    configurations.azuresf.filter { !it.toString().contains("native-preview") }.each {
        from it
    }
    into "lib"
    include "libj*.so", "*.jar"
}

compileJava.dependsOn(explodeDeps)
.
.
.
jar {
    manifest {
        def mpath = configurations.compile.collect {'lib/'+it.getName()}.join (' ')
        mpath = mpath + ' ' + configurations.azuresf.collect {'lib/'+it.getName()}.join (' ')
        attributes(
                'Main-Class': 'reliableactor.MyactorHost',
                "Class-Path": mpath)
    baseName "myactor"
    destinationDir = file('../myjavaapp/MyactorPkg/Code')}
 }
.
.
.
task copyDeps<< {
      copy {
              from("lib/")
              into("../myjavaapp/MyactorPkg/Code/lib")
              include('*')
      }
      copy {
              from("../MyactorInterface/out/lib")
              into("../myjavaapp/MyactorPkg/Code/lib")
              include('*.jar')
      }
}
```

#### <a name="updating-build-script-for-the-test-client-project"></a><span data-ttu-id="447bd-148">Uppdatera byggskript för testklientprojektet</span><span class="sxs-lookup"><span data-stu-id="447bd-148">Updating build script for the test client project</span></span>

<span data-ttu-id="447bd-149">Ändringar här liknar de ändringar som beskrivs i föregående avsnitt, det vill säga aktörsprojektet.</span><span class="sxs-lookup"><span data-stu-id="447bd-149">Changes here are similar to the changes discussed in previous section, that is, the actor project.</span></span> <span data-ttu-id="447bd-150">Tidigare såg Gradle-skriptet ut så här -</span><span class="sxs-lookup"><span data-stu-id="447bd-150">Previously the Gradle script used to be like as follows -</span></span>
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
      compile project(':MyactorInterface')
}
.
.
.
jar
{
    manifest {
    attributes(
        'Main-Class': 'reliableactor.test.MyactorTestClient',
        "Class-Path": configurations.compile.collect { 'lib/' + it.getName() }.join(' '))
    }
    baseName "myactor-test"
  destinationDir = file('out/lib')
}
.
.
.
task copyDeps<< {
        copy {
                from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
                into("./out/lib/lib")
                include('*.jar')
        }
        copy {
                from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
                into("./out/lib/lib")
                include('libj*.so')
        }
        copy {
                from("../MyactorInterface/out/lib")
                into("./out/lib/lib")
                include('*.jar')
        }
}
```
<span data-ttu-id="447bd-151">För att hämta beroendena från Maven skulle **uppdaterade** ``build.gradle`` nu ha motsvarande delar enligt följande -</span><span class="sxs-lookup"><span data-stu-id="447bd-151">Now, to fetch the dependencies from Maven, the **updated** ``build.gradle`` would have the corresponding parts as follows -</span></span>
```
repositories {
    mavenCentral()
}

configurations {
    azuresf
}

dependencies {
    compile project(':MyactorInterface')
    azuresf ('com.microsoft.servicefabric:sf-actors-preview:0.10.0')
    compile fileTree(dir: 'lib', include: '*.jar')
}

task explodeDeps(type: Copy, dependsOn:configurations.azuresf) { task ->
    configurations.azuresf.filter { it.toString().contains("native-preview") }.each{
        from zipTree(it)
    }
    configurations.azuresf.filter { !it.toString().contains("native-preview") }.each {
        from it
    }
    into "lib"
    include "libj*.so", "*.jar"
}

compileJava.dependsOn(explodeDeps)
.
.
.
jar
{
    manifest {
        def mpath = configurations.compile.collect {'lib/'+it.getName()}.join (' ')
        mpath = mpath + ' ' + configurations.azuresf.collect {'lib/'+it.getName()}.join (' ')
    attributes(
                'Main-Class': 'reliableactor.test.MyactorTestClient',
                "Class-Path": mpath)
    baseName "myactor-test"
    destinationDir = file('./out/lib')
        }
}
.
.
.
task copyDeps<< {
        copy {
                from("lib/")
                into("./out/lib/lib")
                include('*')
        }
        copy {
                from("../MyactorInterface/out/lib")
                into("./out/lib/lib")
                include('*.jar')
        }
}
```

## <a name="next-steps"></a><span data-ttu-id="447bd-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="447bd-152">Next steps</span></span>

* [<span data-ttu-id="447bd-153">Skapa och distribuera ditt första Service Fabric-program med Java i Linux med hjälp av Yeoman</span><span class="sxs-lookup"><span data-stu-id="447bd-153">Create and deploy your first Service Fabric Java application on Linux by using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="447bd-154">Skapa och distribuera ditt första Service Fabric-program med Java i Linux med Service Fabric-plugin-programmet för Eclipse</span><span class="sxs-lookup"><span data-stu-id="447bd-154">Create and deploy your first Service Fabric Java application on Linux by using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="447bd-155">Interagera med Service Fabric-kluster med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="447bd-155">Interact with Service Fabric clusters using the Service Fabric CLI</span></span>](service-fabric-cli.md)
