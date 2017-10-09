---
title: "aaaMigrate från Java SDK tooMaven - uppdatera gamla Java-program för Azure Service Fabric toouse Maven | Microsoft Docs"
description: "Uppdatera hello äldre Java-program som används för toouse hello Service Fabric Java SDK toofetch Service Fabric Java beroenden från Maven. När du har slutfört den här installationen är äldre Java-program kan toobuild."
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
ms.openlocfilehash: 11b979facd7b3865141a6d3a035a6021dd06ca0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-your-previous-java-service-fabric-application-toofetch-java-libraries-from-maven"></a><span data-ttu-id="df106-104">Uppdatera din tidigare Java Service Fabric application toofetch Java-bibliotek från Maven</span><span class="sxs-lookup"><span data-stu-id="df106-104">Update your previous Java Service Fabric application toofetch Java libraries from Maven</span></span>
<span data-ttu-id="df106-105">Vi har flyttat Service Fabric Java binärfiler nyligen hello Service Fabric Java SDK tooMaven vara värd för.</span><span class="sxs-lookup"><span data-stu-id="df106-105">We have recently moved Service Fabric Java binaries from hello Service Fabric Java SDK tooMaven hosting.</span></span> <span data-ttu-id="df106-106">Nu kan du använda **mavencentral** toofetch hello senaste Service Fabric Java beroenden.</span><span class="sxs-lookup"><span data-stu-id="df106-106">Now you can use **mavencentral** toofetch hello latest Service Fabric Java dependencies.</span></span> <span data-ttu-id="df106-107">Den här Snabbkurs hjälper du uppdatera din befintliga Java-program som du tidigare har skapat toobe användas med Service Fabric Java SDK med hjälp av antingen Yeoman mall eller Eclipse toobe som är kompatibel med hello Maven baserat build.</span><span class="sxs-lookup"><span data-stu-id="df106-107">This quick-start helps you update your existing Java applications, which you earlier created toobe used with Service Fabric Java SDK, using either Yeoman template or Eclipse, toobe compatible with hello Maven based build.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df106-108">Krav</span><span class="sxs-lookup"><span data-stu-id="df106-108">Prerequisites</span></span>
1. <span data-ttu-id="df106-109">Du måste först toouninstall hello befintliga Java SDK.</span><span class="sxs-lookup"><span data-stu-id="df106-109">First you need toouninstall hello existing Java SDK.</span></span>

  ```bash
  sudo dpkg -r servicefabricsdkjava
  ```
2. <span data-ttu-id="df106-110">Installera hello senaste Service Fabric CLI följande hello anvisningarna [här](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="df106-110">Install hello latest Service Fabric CLI following hello steps mentioned [here](service-fabric-cli.md).</span></span>

3. <span data-ttu-id="df106-111">toobuild och arbete på hello Service Fabric Java-program, måste du tooensure att JDK 1.8 och Gradle är installerade.</span><span class="sxs-lookup"><span data-stu-id="df106-111">toobuild and work on hello Service Fabric Java applications, you need tooensure that you have JDK 1.8 and Gradle installed.</span></span> <span data-ttu-id="df106-112">Om du ännu inte har installerats kan du köra hello följande tooinstall JDK 1.8 (openjdk-8-jdk) och Gradle -</span><span class="sxs-lookup"><span data-stu-id="df106-112">If not yet installed, you can run hello following tooinstall JDK 1.8 (openjdk-8-jdk) and Gradle -</span></span>

 ```bash
 sudo apt-get install openjdk-8-jdk-headless
 sudo apt-get install gradle
 ```
4. <span data-ttu-id="df106-113">Hello installera eller avinstallera uppdateringsskript av ditt program toouse hello nya Service Fabric CLI följa anvisningarna hello [här](service-fabric-application-lifecycle-sfctl.md).</span><span class="sxs-lookup"><span data-stu-id="df106-113">Update hello install/uninstall scripts of your application toouse hello new Service Fabric CLI following hello steps mentioned [here](service-fabric-application-lifecycle-sfctl.md).</span></span> <span data-ttu-id="df106-114">Du kan se tooour Kom igång- [exempel](https://github.com/Azure-Samples/service-fabric-java-getting-started) referens.</span><span class="sxs-lookup"><span data-stu-id="df106-114">You can refer tooour getting-started [examples](https://github.com/Azure-Samples/service-fabric-java-getting-started) for reference.</span></span>

>[!TIP]
> <span data-ttu-id="df106-115">När du har avinstallerat hello Service Fabric Java SDK fungerar inte Yeoman.</span><span class="sxs-lookup"><span data-stu-id="df106-115">After uninstalling hello Service Fabric Java SDK, Yeoman will not work.</span></span> <span data-ttu-id="df106-116">Följ hello krav anges [här](service-fabric-create-your-first-linux-application-with-java.md) toohave Service Fabric Yeoman Java mallen generator upp och fungerar.</span><span class="sxs-lookup"><span data-stu-id="df106-116">Follow hello Prerequisites mentioned [here](service-fabric-create-your-first-linux-application-with-java.md) toohave Service Fabric Yeoman Java template generator up and working.</span></span>

## <a name="service-fabric-java-libraries-on-maven"></a><span data-ttu-id="df106-117">Service Fabric Java-bibliotek på Maven</span><span class="sxs-lookup"><span data-stu-id="df106-117">Service Fabric Java libraries on Maven</span></span>
<span data-ttu-id="df106-118">Service Fabric Java-bibliotek har lagrats i Maven.</span><span class="sxs-lookup"><span data-stu-id="df106-118">Service Fabric Java libraries have been hosted in Maven.</span></span> <span data-ttu-id="df106-119">Du kan lägga till hello beroenden i hello ``pom.xml`` eller ``build.gradle`` av projekt toouse Service Fabric Java-bibliotek från **mavenCentral**.</span><span class="sxs-lookup"><span data-stu-id="df106-119">You can add hello dependencies in hello ``pom.xml`` or ``build.gradle`` of your projects toouse Service Fabric Java libraries from **mavenCentral**.</span></span>

### <a name="actors"></a><span data-ttu-id="df106-120">Aktörer</span><span class="sxs-lookup"><span data-stu-id="df106-120">Actors</span></span>

<span data-ttu-id="df106-121">Service Fabric-stöd för tillförlitliga aktörer för ditt program.</span><span class="sxs-lookup"><span data-stu-id="df106-121">Service Fabric Reliable Actor support for your application.</span></span>

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

### <a name="services"></a><span data-ttu-id="df106-122">Tjänster</span><span class="sxs-lookup"><span data-stu-id="df106-122">Services</span></span>

<span data-ttu-id="df106-123">Service Fabric-stöd för tillståndslös tjänst för ditt program.</span><span class="sxs-lookup"><span data-stu-id="df106-123">Service Fabric Stateless Service support for your application.</span></span>

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

### <a name="others"></a><span data-ttu-id="df106-124">Andra</span><span class="sxs-lookup"><span data-stu-id="df106-124">Others</span></span>
#### <a name="transport"></a><span data-ttu-id="df106-125">Transport</span><span class="sxs-lookup"><span data-stu-id="df106-125">Transport</span></span>

<span data-ttu-id="df106-126">Transportnivåstöd för Service Fabric Java-program.</span><span class="sxs-lookup"><span data-stu-id="df106-126">Transport layer support for Service Fabric Java application.</span></span> <span data-ttu-id="df106-127">Du behöver inte tooexplicitly lägga till den här beroende tooyour tillförlitliga aktören eller tjänstprogram, såvida inte du programmerar på hello Transportskiktet.</span><span class="sxs-lookup"><span data-stu-id="df106-127">You do not need tooexplicitly add this dependency tooyour Reliable Actor or Service applications, unless you program at hello transport layer.</span></span>

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

#### <a name="fabric-support"></a><span data-ttu-id="df106-128">Fabric-stöd</span><span class="sxs-lookup"><span data-stu-id="df106-128">Fabric support</span></span>

<span data-ttu-id="df106-129">Nivån stöd för Service Fabric som nämns toonative Service Fabric runtime.</span><span class="sxs-lookup"><span data-stu-id="df106-129">System level support for Service Fabric, which talks toonative Service Fabric runtime.</span></span> <span data-ttu-id="df106-130">Du behöver inte tooexplicitly lägga till det här beroendet tooyour tillförlitliga aktören eller tjänstprogram.</span><span class="sxs-lookup"><span data-stu-id="df106-130">You do not need tooexplicitly add this dependency tooyour Reliable Actor or Service applications.</span></span> <span data-ttu-id="df106-131">Detta hämtar hämtats automatiskt från Maven, när du inkluderar hello andra beroenden som ovan.</span><span class="sxs-lookup"><span data-stu-id="df106-131">This gets fetched automatically from Maven, when you include hello other dependencies above.</span></span>

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


## <a name="migrating-service-fabric-stateless-service"></a><span data-ttu-id="df106-132">Migrera tillståndslös Service Fabric-tjänst</span><span class="sxs-lookup"><span data-stu-id="df106-132">Migrating Service Fabric Stateless Service</span></span>

<span data-ttu-id="df106-133">toobe kan toobuild din befintliga Service Fabric tillståndslös Java tjänsten med hjälp av Service Fabric-beroenden som hämtats från Maven måste tooupdate hello ``build.gradle`` fil i hello Service.</span><span class="sxs-lookup"><span data-stu-id="df106-133">toobe able toobuild your existing Service Fabric stateless Java service using Service Fabric dependencies fetched from Maven, you need tooupdate hello ``build.gradle`` file inside hello Service.</span></span> <span data-ttu-id="df106-134">Tidigare har använts den toobe som följande-</span><span class="sxs-lookup"><span data-stu-id="df106-134">Previously it used toobe like as follows -</span></span>
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
<span data-ttu-id="df106-135">Nu toofetch hello beroenden från Maven, hello **uppdateras** ``build.gradle`` skulle ha hello motsvarande delar enligt följande -</span><span class="sxs-lookup"><span data-stu-id="df106-135">Now, toofetch hello dependencies from Maven, hello **updated** ``build.gradle`` would have hello corresponding parts as follows -</span></span>
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
<span data-ttu-id="df106-136">I allmänhet tooget en allmän uppfattning om hur du skapar skript hello skulle se ut för en tillståndslös Java-tjänst för Service Fabric kan du kontrollera tooany exempel från våra Kom igång-exempel.</span><span class="sxs-lookup"><span data-stu-id="df106-136">In general, tooget an overall idea about how hello build script would look like for a Service Fabric stateless Java service, you can refer tooany sample from our getting-started examples.</span></span> <span data-ttu-id="df106-137">Här är hello [build.gradle](https://github.com/Azure-Samples/service-fabric-java-getting-started/blob/master/Services/EchoServer/EchoServer1.0/EchoServerService/build.gradle) hello EchoServer exempel.</span><span class="sxs-lookup"><span data-stu-id="df106-137">Here is hello [build.gradle](https://github.com/Azure-Samples/service-fabric-java-getting-started/blob/master/Services/EchoServer/EchoServer1.0/EchoServerService/build.gradle) for hello EchoServer sample.</span></span>

## <a name="migrating-service-fabric-actor-service"></a><span data-ttu-id="df106-138">Migrera Service Fabric-aktörstjänst</span><span class="sxs-lookup"><span data-stu-id="df106-138">Migrating Service Fabric Actor Service</span></span>

<span data-ttu-id="df106-139">toobe kan toobuild din befintliga Service Fabric aktören Java-program med hjälp av Service Fabric-beroenden som hämtats från Maven måste tooupdate hello ``build.gradle`` filen inuti hello gränssnittet paketet och hello Service-paketet.</span><span class="sxs-lookup"><span data-stu-id="df106-139">toobe able toobuild your existing Service Fabric Actor Java application using Service Fabric dependencies fetched from Maven, you need tooupdate hello ``build.gradle`` file inside hello interface package and in hello Service package.</span></span> <span data-ttu-id="df106-140">Om du har ett TestClient-paket måste tooupdate som också.</span><span class="sxs-lookup"><span data-stu-id="df106-140">If you have a TestClient package, you need tooupdate that as well.</span></span> <span data-ttu-id="df106-141">I så fall för din aktören ``Myactor``, hello följande skulle vara hello platser där du behöver tooupdate -</span><span class="sxs-lookup"><span data-stu-id="df106-141">So, for your actor ``Myactor``, hello following would be hello places where you need tooupdate -</span></span>
```
./Myactor/build.gradle
./MyactorInterface/build.gradle
./MyactorTestClient/build.gradle
```

#### <a name="updating-build-script-for-hello-interface-project"></a><span data-ttu-id="df106-142">Uppdatera build-skript för hello gränssnittet projekt</span><span class="sxs-lookup"><span data-stu-id="df106-142">Updating build script for hello interface project</span></span>

<span data-ttu-id="df106-143">Tidigare har använts den toobe som följande-</span><span class="sxs-lookup"><span data-stu-id="df106-143">Previously it used toobe like as follows -</span></span>
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
}
.
.
```
<span data-ttu-id="df106-144">Nu toofetch hello beroenden från Maven, hello **uppdateras** ``build.gradle`` skulle ha hello motsvarande delar enligt följande -</span><span class="sxs-lookup"><span data-stu-id="df106-144">Now, toofetch hello dependencies from Maven, hello **updated** ``build.gradle`` would have hello corresponding parts as follows -</span></span>
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

#### <a name="updating-build-script-for-hello-actor-project"></a><span data-ttu-id="df106-145">Uppdatera build-skript för hello aktören projekt</span><span class="sxs-lookup"><span data-stu-id="df106-145">Updating build script for hello actor project</span></span>

<span data-ttu-id="df106-146">Tidigare har använts den toobe som följande-</span><span class="sxs-lookup"><span data-stu-id="df106-146">Previously it used toobe like as follows -</span></span>
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
<span data-ttu-id="df106-147">Nu toofetch hello beroenden från Maven, hello **uppdateras** ``build.gradle`` skulle ha hello motsvarande delar enligt följande -</span><span class="sxs-lookup"><span data-stu-id="df106-147">Now, toofetch hello dependencies from Maven, hello **updated** ``build.gradle`` would have hello corresponding parts as follows -</span></span>
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

#### <a name="updating-build-script-for-hello-test-client-project"></a><span data-ttu-id="df106-148">Uppdatera build-skript för hello test klientprojektet</span><span class="sxs-lookup"><span data-stu-id="df106-148">Updating build script for hello test client project</span></span>

<span data-ttu-id="df106-149">Här ändringar är liknande toohello ändringar som beskrivs i föregående avsnitt, det vill säga hello aktören projektet.</span><span class="sxs-lookup"><span data-stu-id="df106-149">Changes here are similar toohello changes discussed in previous section, that is, hello actor project.</span></span> <span data-ttu-id="df106-150">Tidigare hello Gradle-skriptet som används toobe som följande-</span><span class="sxs-lookup"><span data-stu-id="df106-150">Previously hello Gradle script used toobe like as follows -</span></span>
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
<span data-ttu-id="df106-151">Nu toofetch hello beroenden från Maven, hello **uppdateras** ``build.gradle`` skulle ha hello motsvarande delar enligt följande -</span><span class="sxs-lookup"><span data-stu-id="df106-151">Now, toofetch hello dependencies from Maven, hello **updated** ``build.gradle`` would have hello corresponding parts as follows -</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="df106-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="df106-152">Next steps</span></span>

* [<span data-ttu-id="df106-153">Skapa och distribuera ditt första Service Fabric-program med Java i Linux med hjälp av Yeoman</span><span class="sxs-lookup"><span data-stu-id="df106-153">Create and deploy your first Service Fabric Java application on Linux by using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="df106-154">Skapa och distribuera ditt första Service Fabric-program med Java i Linux med Service Fabric-plugin-programmet för Eclipse</span><span class="sxs-lookup"><span data-stu-id="df106-154">Create and deploy your first Service Fabric Java application on Linux by using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="df106-155">Interagera med Service Fabric-kluster med hello Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="df106-155">Interact with Service Fabric clusters using hello Service Fabric CLI</span></span>](service-fabric-cli.md)
